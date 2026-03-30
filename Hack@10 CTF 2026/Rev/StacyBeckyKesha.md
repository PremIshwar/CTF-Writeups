# Is It Stacy, Is It Becky, Is It Kesha?
_Can you find out who's email address has access?
Kindly submit it in the form of HACK10{email@domain.com}_
\
\
Another .exe reverse engineering challenge. I dedcided to opent this on in dnSpy since it is a .NET application.
\
\
<img width="1021" height="80" alt="image" src="https://github.com/user-attachments/assets/36d5c7e9-96f0-47a5-b3ad-13c7566c1dfa" />
\
\
The main function had a wordy variable but it kinda looked like a fake flag. I decided to focus on this part of the main function.
<br><br>
```C#
Console.Write("Enter email address: ");
string email = Console.ReadLine().Trim().ToLower();
bool flag = string.IsNullOrEmpty(email);
if (flag)
{
  Console.WriteLine("No email entered. Exiting.");
}
else
{
  bool flag2 = await Program.CheckEmailExists(email);
  bool exists = flag2;
  if (exists)
  {
    Console.WriteLine("\nUser exists, but you guessed the wrong one");
  }
  else
  {
    Console.WriteLine("\nUser doesn't exist");
  }
  string hash = Program.HashEmail(email);
  if (hash == "0d103375d4f99df6bc92a931aa8f48b1")
  {
    Console.ForegroundColor = ConsoleColor.Green;
    Console.WriteLine("\nYou found the flag! Now submit the flag as HACK10{" + email + "}");
    Console.ResetColor();
  }
  Console.WriteLine("\nPress any key to exit...");
  Console.ReadKey();
```
\
\
The program gets an email entered by the user and passes it through the CheckEmailExists() function. It then passes the email through the HashEmail() function and compares it with a hardcoded hash and if there is a match, it prints the flag (which is the email of the hash). Let's take a look at CheckEmailExists().
<br><br>
```C#
string text = await client.GetStringAsync("https://appsecmy.com/d22646ad92dfaa334f9fa1c3579b4801.txt");
string content = text;
text = null;
string[] lines = content.Split(new char[] { '\r', '\n' }, StringSplitOptions.RemoveEmptyEntries);
foreach (string line in lines)
{
  if (line.Trim().ToLower() == email)
  {
    return true;
  }
  line = null;
}
string[] array = null;
flag = false;
```
\
\
Here is a snippet of the function. All this function does is check if the email is in a [email list] (https://appsecmy.com/d22646ad92dfaa334f9fa1c3579b4801.txt). We can actually view this email list as it public. Now let's look at the HashEmail() function.
<br><br>
```C#
private static string HashEmail(string email)
{
string text;
using (MD5 md = MD5.Create())
{
  byte[] bytes = Encoding.UTF8.GetBytes(email);
  byte[] array = md.ComputeHash(bytes);
  StringBuilder stringBuilder = new StringBuilder();
  foreach (byte b in array)
  {
    stringBuilder.Append(b.ToString("x2"));
  }
  text = stringBuilder.ToString();
}
return text;
}
```
This seems to be just using MD5 hashing. Now, we can actually easily figure out what email is the flag. I downloaded the email list and saved the hardcoed hash into ```hashes.txt```. Then, I used JohnTheRipper to crack the hash with the email list as the wordlist.
\
\
<img width="975" height="298" alt="image" src="https://github.com/user-attachments/assets/4cfb4b94-e3bf-4d73-9e6c-6553dca92670" />
\
\
Flag: ```HACK10{wa00d6d88epd0z1x6gro@rediffmail.com}```
