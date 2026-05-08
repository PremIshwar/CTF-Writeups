# Introduction to Chinese OSINT Writeup

Follow me as I complete the Hacktoria Introduction to Chinese OSINT Certification. This is a collection of fun OSINT tasks focusing on Chinese Open-Source Intelligence and it's pretty beginner friendly. Let's dive right in!

<br>

## OBJECTIVE 1 — Source Identification

*Using ONLY Chinese search terms, locate the blog distributing t00ls invitation codes. Search using Chinese characters only — no English, no pinyin. The forum name is t00ls. The Chinese term for invitation code is 邀请码. Your search will return multiple results. Verify by checking for active user engagement in comments and multiple pages of requests.*

*TO UNLOCK NEXT STEP ->   What is the top-level domain (TLD)? (English, no caps)*



I decided to first translate what the Chinese characters meant.


<p align="center">
<img width="650" height="200" alt="image" src="https://github.com/user-attachments/assets/59afd5f6-10b9-4692-afdc-d5e6d46f53da" />
</p>


We can try a simple Google search of "邀请码 t00ls" and see if that returns anything.


<p align="center">
<img width="500" height="154" alt="image" src="https://github.com/user-attachments/assets/bf060064-7e6a-4e7d-95f3-5328fcc5518a" />
</p>


This was the first result: https://huaidan.org/archives/3408.html. The password for the next objective file is ```org```

<br>

## OBJECTIVE 2 — Target Identification

*The comments section spans multiple pages. Your target left their request in February 2017. Their message poetically references fate — meeting someone special in a vast sea of people. They requested a code from "鬼哥" (Brother Ghost).*

*Navigate to locate this specific comment.*

*TO UNLOCK NEXT STEP ->   What is the full QQ email address they exposed? (English)*

Within the blog, we can navigate to the page with comments from 2017. There were only 2 from February.


<p align="center">
<img width="668" height="289" alt="image" src="https://github.com/user-attachments/assets/0fe42e81-2b22-4bdb-9225-d501fda4fa2f" />
</p>


The password for the next objective file is ```646891950@qq.com```

<br>

## OBJECTIVE 3 — Platform Pivot via Search

*Now Search. The target has used this same contact information elsewhere on the Chinese internet — specifically on a major Chinese Q&A platform similar to Yahoo Answers.*

*TO UNLOCK NEXT STEP ->   What is the name of this platform where you find their activity? (Chinese)*

After a quick search online, I found that one of the largest Q&A platforms in China was Baidu Knows (Chinese: 百度知道; pinyin: Bǎidù Zhidao). To confirm, I did some Google Dorking to see if I could find any occurrences of the email addresss online.


<p align="center">
<img width="600" height="200" alt="image" src="https://github.com/user-attachments/assets/89877e51-eb63-4ae1-9861-4f24a3f99215" />
</p>


There was an occurance in Baidu Zhidao: https://zhidao.baidu.com/question/463893160/answer/1152589012.html?&mzl=qb_xg_3&fr=relate&word=&refer_title=%E6%B1%82%E5%9B%9E%E9%9F%B3%E5%93%A5%E7%BF%BB%E5%94%B1%E7%9A%84%E5%85%A8%E9%83%A8%E6%AD%8C%E6%9B%B2~MP3%E7%9A%84&

The password for the next objective file is ```百度知道```. NOTE: This password actually does not work on the installed version of Windows 7-Zip. It does work in the web version of 7-Zip and unzipping in Linux.

<br>

## OBJECTIVE 4 — Profile Analysis

*The target has answered over 500 questions on this platform. Their answers reveal extensive knowledge of a specific Chinese city — they discuss metro lines, districts, rental prices, local landmarks, and even which neighborhoods are cheaper. They mention working 12-hour days in a specific district.*

*TO UNLOCK NEXT STEP ->   Based on analyzing their answer history, what Chinese city does this person clearly live and work in? (English)*

Going to ther user profile, we can see all the questions they have posted. 


<p align="center">
<img width="800" height="200" alt="image" src="https://github.com/user-attachments/assets/30c8b050-dd1f-40f0-8f94-da356dd22b85" />
</p>


Doing a search in the webpage for "12-hour", we can find the post mentioned in the objective. A quick search reveals that Yuhang is a district of Hangzhou City.
The password for the next objective file is ```Hangzhou```

<br>

# OBJECTIVE 5 — Work Profile

*In one of their answers, the target directly mentions their work situation — their district, salary, and hours. They also answer many technical questions about a specific field.*

*TO UNLOCK NEXT STEP ->   What professional field does this person work in? (Two letters, common abbreviation, English)*

This user actually answered a lot of questions on Python and other general computer related stuff.


<p align="center">
<img width="800" height="300" alt="image" src="https://github.com/user-attachments/assets/cfbcaecb-17ef-4941-9a84-88d2d0e7a1e4" />
</p>


The password for the next objective file is ```IT```

<br>

## OBJECTIVE 6 — QQ to Mobile Pivot

*Now pivot from the QQ ID to a mobile number. The QQ ID can be extracted from the email format: [QQ_ID]@qq.com*

*SERPENT previously obtained partial infrastructure data from a Chinese OSINT operator during a joint signals operation. The following fragment was recovered from a seized asset list:*

*SOURCE: [__]SOV*
*STATUS: ACTIVE*
*ENDPOINT: [REDACTED]_tel.html*
*CAPABILITY: QQ ↔ MOBILE CORRELATION*

*Reconstruct the source domain and endpoint. The endpoint pattern is [PLATFORM]_tel.html where platform is a two-letter abbreviation.*

*TO UNLOCK NEXT STEP ->   What is the linked mobile number? (11 digits)*

I simply made a guess that the 2 letter abbreviation would be "qq" and searched for qq_tel.html


<p align="center">
<img width="724" height="400" alt="image" src="https://github.com/user-attachments/assets/f103adf7-3b90-4af7-9251-d49f1d9c7cd2" />
</p>


Using the hints given in the objective, we can deduce that the platform is https://avsov.com/qq_tel.html. This platform actually lets us look up QQ IDs.


<p align="center">
<img width="489" height="213" alt="image" src="https://github.com/user-attachments/assets/670e2780-31a9-4d0d-b55b-a63ad4dbc741" />
</p>


The password for the next objective file is ```15158036671```

<br>

## OBJECTIVE 7 — Mobile to Weibo Pivot

*Using the same infrastructure, pivot from mobile number to Weibo identity. A secondary endpoint exists:*

*ENDPOINT: tel_[REDACTED].html
CAPABILITY: MOBILE ↔ WEIBO CORRELATION*

*The endpoint pattern is tel_[PLATFORM].html where platform is a two-letter abbreviation for the Chinese social media platform.*

*Input the mobile number. The tool may return multiple associated IDs.*

*TO UNLOCK NEXT STEP ->   What is the Weibo-associated ID returned? (Only the 10 digits)*

Since we're looking for a Weibo ID, I decided to try https://avsov.com/tel_wb.html and that worked!


<p align="center">
<img width="480" height="210" alt="image" src="https://github.com/user-attachments/assets/6001a901-4218-4043-ad4f-1e6cf5fc1199" />
</p>


The password for the next objective file is ```1695825683```

<br>

## OBJECTIVE 8 — WeChat Verification

*Use the mobile number to search for the associated WeChat account. WeChat allows searching users by phone number.*

*The WeChat profile displays a region. Can you verify that the data shown in the Baidu profile matches the one on WeChat?*

*TO UNLOCK NEXT STEP ->   What specific city is shown in their WeChat profile region? (English)*

After looking for publicly available tools online to lookup WeChat account. After several deadends, I decided to surrender and download the WeChat app to look up their phone number.


<p align="center">
<img width="600" height="400" alt="image" src="https://github.com/user-attachments/assets/6651f520-9377-48f2-bce2-00f675cb2300" />
</p>

The password for the next objective file is ```Hangzhou```

<br>


## OBJECTIVE 9 — Cross-Reference Verification

*You now have location data from three independent sources:*

*Their Q&A answers (mentioned city, districts, landmarks)
Mobile number prefix (indicates registration province)
WeChat profile region*

*The mobile number prefix 151 is assigned to China Mobile. The following digits 580 indicate Zhejiang province registration. Does all evidence point to the same location?*

*TO UNLOCK NEXT STEP ->   What province does this person reside in? (English)*

We can get the province from their WeChat profile. The password for the next objective file is ```Zhejiang```

<br>

## OBJECTIVE 10 — Alternative Access

*Beyond invitation codes, t00ls has implemented an alternative access mechanism. Search for recent t00ls news in Chinese. The method involves blockchain technology and represents a monetization shift common among Chinese underground forums.*

*TO COMPLETE THE CTF ->   What blockchain-based method now provides access to t00ls forum? (3 letters, English, all caps)*

While looking at the t00ls website in the earlier objective, I remembered seeing a mention of NFTs. "NFT" seems to fit into the requirements of our objectives, so I decided to do some dorking to confirm my hunch.


<p align="center">
<img width="882" height="279" alt="image" src="https://github.com/user-attachments/assets/e0006c76-5bf5-41a6-bb98-f797e97a8429" />
</p>


Translating this page tells use that t00ls launched some NFTs, that if bought, allows some non-members to access t00ls.


<p align="center">
<img width="433" height="301" alt="image" src="https://github.com/user-attachments/assets/cb029400-a446-46c1-83cb-52339994489a" />
</p>

The password for the next objective file is ```NFT```

That's the end of Introduction to Chinese OSINT by Hakctoria. Thanks for reading!

