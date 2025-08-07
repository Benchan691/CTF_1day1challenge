Solving picoCTF: findme
Challenge Description
The challenge involves a web application with a login form. The goal is to find the flag by exploring redirections after logging in. The hint suggests checking for any redirections.
The website is hosted at http://saturn.picoctf.net:53130/. Help test the form by submitting the username test and password test!.
Tools Used

Burp Suite: Burp Suite is a comprehensive web vulnerability scanner and proxy tool developed by PortSwigger. It allows users to intercept, inspect, and modify HTTP/S requests and responses between a browser and a web server. It is commonly used for security testing, debugging web applications, and capturing redirection chains that may not be visible in a standard browser session.

Solution Steps
Step 1: Set Up Burp Suite Proxy

Launch Burp Suite and configure the proxy listener (default is 127.0.0.1:8080).
Set your browser's proxy settings to route traffic through Burp Suite.
Enable the proxy interception if needed, but for this challenge, focus on the HTTP history tab to capture all requests and responses.

Step 2: Access the Website and Login

Navigate to http://saturn.picoctf.net:53130/ in your proxy-enabled browser.
You will see a login form.
Enter the username test and password test!.
Submit the form.

Step 3: Observe Redirections in Burp Suite

After submitting the login form, the application performs a series of redirections.
In Burp Suite, switch to the Proxy > HTTP history tab.
Look for the POST request to /login, which returns a 302 status code (redirect).
Follow the chain of requests. The redirections lead to pages like /home and multiple /next-page/id=<base64_string>.
From the provided screenshot, the relevant requests include:

GET /next-page/id=bF90aGVfd2Ffd2Ffd2F3... (200 OK)
GET /next-page/id=cGljb0NURnt0aGVfd2Ffd2Ffd2F3... (200 OK)
Note: The table in the screenshot is sorted by status code, so the chronological order may differ. The actual redirection chain starts with the login POST and follows the Location headers.



Step 4: Extract and Concatenate the Base64-Encoded IDs

Extract the id parameters from the redirection URLs in the history. These are base64-encoded strings that form parts of the flag.
The IDs from the screenshot are:

First part (starting with picoCTF): cGljb0NURnt0aGVfd2Ffd2Ffd2F3...
Second part: bF90aGVfd2Ffd2Ffd2F3...


Concatenate these base64 strings in the order they appear in the redirection chain (typically the part starting with cGljb0NURn... first, followed by the bF90aGVf... part to form a complete base64 string).

Step 5: Decode the Concatenated Base64 String

Take the full concatenated base64 string (ensure it's complete; add padding == if necessary for valid decoding).
Decode it using a base64 decoder (e.g., in Python, CyberChef, or command line with echo <string> | base64 -d).
The decoded string will be the flag in the format picoCTF{...}.

Flag
After decoding the concatenated base64 IDs from the redirections, the flag is obtained. (Note: The exact flag value depends on the instance, but follows the pattern picoCTF{proxies_all_the_way_<random_hex>} based on similar solutions. Ensure the IDs are fully captured and concatenated correctly for your instance.)
Key Takeaway
This challenge teaches the importance of inspecting HTTP redirection chains using tools like Burp Suite, as browsers automatically follow redirects without showing intermediate URLs that may contain critical information. Always check proxy history for hidden details!
