# Dear Hiring Manager
_Umbrella Corporation is currently hiring new employees. One day, the hiring manager opens a resume submitted by a candidate. Moments later, all computers and servers across the company suddenly freeze and become unresponsive.
As a digital forensic investigator, your task is to analyze the incident and determine what happened._
\
\
We are given a PDF file of a resume. There weren't any invisible text that could be selected.
\
\
<img width="693" height="728" alt="image" src="https://github.com/user-attachments/assets/6811e9c0-ebf5-4049-b159-579b0f7e2838" />
\
\
I took a look at the strings of th PDF file and found some weird stuff
\
\
<img width="975" height="311" alt="image" src="https://github.com/user-attachments/assets/d6a919b0-58d2-4ec0-b35d-6183973cc515" />
\
\
I decided to run ```pdfid``` to look for any JavaScript or execution scripts. I found that there was some JavaScript and OpenAction present.
\
\
<img width="414" height="631" alt="image" src="https://github.com/user-attachments/assets/bd0f9900-b7e7-415c-8b59-558da74bc682" />
\
\
I used ```pdfparser``` to exctract these items to inspect them.
\
\
<img width="892" height="103" alt="image" src="https://github.com/user-attachments/assets/fee02163-950d-4cca-a4c7-592f89f87269" />
\
\
This was the JS code I found in the parsed output
```
<<
    /JS '(\\n    var a=["BOPCd","0edrK"," 1i+m"];\\n    var b=["VBeX","U8:","ddd$"];\\n    eval\\(atob\\(a.join\\(""\\)+b.join\\(""\\)\\)\\);\\n  )'
    /S /JavaScript
    /Type /Action
  >>
```
Based on this, we can extract the payload: ```BOPCd0edrK1i+mVBeXU8ddd$```. Putting this into CyberChef (decode from Base85) gives us the flag: ```hack10{M4l1ci0s_PDF}```

