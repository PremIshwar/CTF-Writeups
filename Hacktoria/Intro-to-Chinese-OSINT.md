# Intro-to-Chinese-OSINT

## Introduction to Chinese OSINT Writeup

Follow me as I complete the Hacktoria Introduction to Chinese OSINT Certification. This is a collection of fun OSINT tasks focusing on Chinese Open-Source Intelligence and it's pretty beginner friendly. Let's dive right in!

<br>

### OBJECTIVE 1 — Source Identification

_Using ONLY Chinese search terms, locate the blog distributing t00ls invitation codes. Search using Chinese characters only — no English, no pinyin. The forum name is t00ls. The Chinese term for invitation code is 邀请码. Your search will return multiple results. Verify by checking for active user engagement in comments and multiple pages of requests._

_TO UNLOCK NEXT STEP -> What is the top-level domain (TLD)? (English, no caps)_

I decided to first translate what the Chinese characters meant.

<div align="center"><img src="https://github.com/user-attachments/assets/59afd5f6-10b9-4692-afdc-d5e6d46f53da" alt="image" width="563"></div>

We can try a simple Google search of "邀请码 t00ls" and see if that returns anything.

<div align="center"><img src="https://github.com/user-attachments/assets/bf060064-7e6a-4e7d-95f3-5328fcc5518a" alt="image" width="563"></div>

This was the first result: https://huaidan.org/archives/3408.html. The password for the next objective file is `org`

<br>

### OBJECTIVE 2 — Target Identification

_The comments section spans multiple pages. Your target left their request in February 2017. Their message poetically references fate — meeting someone special in a vast sea of people. They requested a code from "鬼哥" (Brother Ghost)._

_Navigate to locate this specific comment._

_TO UNLOCK NEXT STEP -> What is the full QQ email address they exposed? (English)_

Within the blog, we can navigate to the page with comments from 2017. There were only 2 from February.

<div align="center"><img src="https://github.com/user-attachments/assets/0fe42e81-2b22-4bdb-9225-d501fda4fa2f" alt="image" width="563"></div>

The password for the next objective file is `646891950@qq.com`

<br>

### OBJECTIVE 3 — Platform Pivot via Search

_Now Search. The target has used this same contact information elsewhere on the Chinese internet — specifically on a major Chinese Q\&A platform similar to Yahoo Answers._

_TO UNLOCK NEXT STEP -> What is the name of this platform where you find their activity? (Chinese)_

After a quick search online, I found that one of the largest Q\&A platforms in China was Baidu Knows (Chinese: 百度知道; pinyin: Bǎidù Zhidao). To confirm, I did some Google Dorking to see if I could find any occurrences of the email addresss online.

<div align="center"><img src="https://github.com/user-attachments/assets/89877e51-eb63-4ae1-9861-4f24a3f99215" alt="image" width="563"></div>

There was an occurance in Baidu Zhidao: https://zhidao.baidu.com/question/463893160/answer/1152589012.html?\&mzl=qb\_xg\_3\&fr=relate\&word=\&refer\_title=%E6%B1%82%E5%9B%9E%E9%9F%B3%E5%93%A5%E7%BF%BB%E5%94%B1%E7%9A%84%E5%85%A8%E9%83%A8%E6%AD%8C%E6%9B%B2\~MP3%E7%9A%84&

The password for the next objective file is `百度知道`. NOTE: This password actually does not work on the installed version of Windows 7-Zip. It does work in the web version of 7-Zip and unzipping in Linux.

<br>

### OBJECTIVE 4 — Profile Analysis

_The target has answered over 500 questions on this platform. Their answers reveal extensive knowledge of a specific Chinese city — they discuss metro lines, districts, rental prices, local landmarks, and even which neighborhoods are cheaper. They mention working 12-hour days in a specific district._

_TO UNLOCK NEXT STEP -> Based on analyzing their answer history, what Chinese city does this person clearly live and work in? (English)_

Going to ther user profile, we can see all the questions they have posted.

<div align="center"><img src="https://github.com/user-attachments/assets/30c8b050-dd1f-40f0-8f94-da356dd22b85" alt="image" width="563"></div>

Doing a search in the webpage for "12-hour", we can find the post mentioned in the objective. A quick search reveals that Yuhang is a district of Hangzhou City. The password for the next objective file is `Hangzhou`

<br>

## OBJECTIVE 5 — Work Profile

_In one of their answers, the target directly mentions their work situation — their district, salary, and hours. They also answer many technical questions about a specific field._

_TO UNLOCK NEXT STEP -> What professional field does this person work in? (Two letters, common abbreviation, English)_

This user actually answered a lot of questions on Python and other general computer related stuff.

<div align="center"><img src="https://github.com/user-attachments/assets/cfbcaecb-17ef-4941-9a84-88d2d0e7a1e4" alt="image" width="563"></div>

The password for the next objective file is `IT`

<br>

### OBJECTIVE 6 — QQ to Mobile Pivot

_Now pivot from the QQ ID to a mobile number. The QQ ID can be extracted from the email format: \[QQ\_ID]@qq.com_

_SERPENT previously obtained partial infrastructure data from a Chinese OSINT operator during a joint signals operation. The following fragment was recovered from a seized asset list:_

_SOURCE: \[\_\_]SOV_ _STATUS: ACTIVE_ _ENDPOINT: \[REDACTED]\_tel.html_ _CAPABILITY: QQ ↔ MOBILE CORRELATION_

_Reconstruct the source domain and endpoint. The endpoint pattern is \[PLATFORM]\_tel.html where platform is a two-letter abbreviation._

_TO UNLOCK NEXT STEP -> What is the linked mobile number? (11 digits)_

I simply made a guess that the 2 letter abbreviation would be "qq" and searched for qq\_tel.html

<div align="center"><img src="https://github.com/user-attachments/assets/f103adf7-3b90-4af7-9251-d49f1d9c7cd2" alt="image" width="563"></div>

Using the hints given in the objective, we can deduce that the platform is https://avsov.com/qq\_tel.html. This platform actually lets us look up QQ IDs.

<div align="center"><img src="https://github.com/user-attachments/assets/670e2780-31a9-4d0d-b55b-a63ad4dbc741" alt="image" width="563"></div>

The password for the next objective file is `15158036671`

<br>

### OBJECTIVE 7 — Mobile to Weibo Pivot

_Using the same infrastructure, pivot from mobile number to Weibo identity. A secondary endpoint exists:_

_ENDPOINT: tel\_\[REDACTED].html CAPABILITY: MOBILE ↔ WEIBO CORRELATION_

_The endpoint pattern is tel\_\[PLATFORM].html where platform is a two-letter abbreviation for the Chinese social media platform._

_Input the mobile number. The tool may return multiple associated IDs._

_TO UNLOCK NEXT STEP -> What is the Weibo-associated ID returned? (Only the 10 digits)_

Since we're looking for a Weibo ID, I decided to try https://avsov.com/tel\_wb.html and that worked!

<div align="center"><img src="https://github.com/user-attachments/assets/6001a901-4218-4043-ad4f-1e6cf5fc1199" alt="image" width="563"></div>

The password for the next objective file is `1695825683`

<br>

### OBJECTIVE 8 — WeChat Verification

_Use the mobile number to search for the associated WeChat account. WeChat allows searching users by phone number._

_The WeChat profile displays a region. Can you verify that the data shown in the Baidu profile matches the one on WeChat?_

_TO UNLOCK NEXT STEP -> What specific city is shown in their WeChat profile region? (English)_

After looking for publicly available tools online to lookup WeChat account. After several deadends, I decided to surrender and download the WeChat app to look up their phone number.

<div align="center"><img src="https://github.com/user-attachments/assets/6651f520-9377-48f2-bce2-00f675cb2300" alt="image" width="375"></div>

The password for the next objective file is `Hangzhou`

<br>

### OBJECTIVE 9 — Cross-Reference Verification

_You now have location data from three independent sources:_

_Their Q\&A answers (mentioned city, districts, landmarks) Mobile number prefix (indicates registration province) WeChat profile region_

_The mobile number prefix 151 is assigned to China Mobile. The following digits 580 indicate Zhejiang province registration. Does all evidence point to the same location?_

_TO UNLOCK NEXT STEP -> What province does this person reside in? (English)_

We can get the province from their WeChat profile. The password for the next objective file is `Zhejiang`

<br>

### OBJECTIVE 10 — Alternative Access

_Beyond invitation codes, t00ls has implemented an alternative access mechanism. Search for recent t00ls news in Chinese. The method involves blockchain technology and represents a monetization shift common among Chinese underground forums._

_TO COMPLETE THE CTF -> What blockchain-based method now provides access to t00ls forum? (3 letters, English, all caps)_

While looking at the t00ls website in the earlier objective, I remembered seeing a mention of NFTs. "NFT" seems to fit into the requirements of our objectives, so I decided to do some dorking to confirm my hunch.

<div align="center"><img src="https://github.com/user-attachments/assets/e0006c76-5bf5-41a6-bb98-f797e97a8429" alt="image" width="563"></div>

Translating this page tells use that t00ls launched some NFTs, that if bought, allows some non-members to access t00ls.

<div align="center"><img src="https://github.com/user-attachments/assets/cb029400-a446-46c1-83cb-52339994489a" alt="image" width="563"></div>

The password for the next objective file is `NFT`

That's the end of Introduction to Chinese OSINT by Hakctoria. Thanks for reading!
