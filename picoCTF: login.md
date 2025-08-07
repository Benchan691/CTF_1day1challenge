# PicoCTF Login Challenge Walkthrough

## Overview
The "Login" challenge in picoCTF is a beginner-friendly web exploitation task worth 100 points. It involves inspecting the source code of a login page to uncover a hidden flag encoded in Base64 within a JavaScript file. The challenge tricks participants into thinking it's about SQL injection or credential guessing, but the solution lies in client-side code analysis.

## Prerequisites
- Basic knowledge of web development tools (e.g., browser Developer Tools).
- A Base64 decoder (online tools like base64decode.org or JavaScript console with `atob()`).

## Steps to Solve

### Step 1: Access the Challenge
Navigate to the provided challenge URL (typically something like `https://play.picoctf.org/practice/challenge/<id>`). You'll see a standard login page with fields for username and password. Attempting to log in with random credentials may not yield results, and SQL injection payloads (e.g., from common lists) won't work here.

### Step 2: Inspect the HTML Source
Right-click on the page and select "View Page Source" (or press Ctrl+U). Scan the HTML for any hints or comments. In this challenge, there are no obvious flags or hints in the HTML itself, but note any linked resources, such as a JavaScript file (often named `index.js` or similar).

### Step 3: Examine the JavaScript File
Locate the `<script>` tag in the HTML that references an external JS file. Open it by clicking the link in the source view or using the browser's Network tab in Developer Tools (F12). In the JS code, look for functions related to encoding/decoding, such as `btoa()` (Base64 encode) or `atob()` (Base64 decode). You'll find a suspicious Base64-encoded string, for example:  
`cGljb0NURns1M3J2M3JfNTNydjNyXzUzcnYzcl81M3J2M3JfNTNydjNyfQ`  
This string is part of the code but not executed in a way that reveals the flag directly.

### Step 4: Decode the Base64 String
Copy the encoded string and decode it using:
- An online decoder (e.g., https://www.base64decode.org/).
- JavaScript console: Open the console in Developer Tools and run `atob('cGljb0NURns1M3J2M3JfNTNydjNyXzUzcnYzcl81M3J2M3JfNTNydjNyfQ')`.
- Command line: `echo "cGljb0NURns1M3J2M3JfNTNydjNyXzUzcnYzcl81M3J2M3JfNTNydjNyfQ==" | base64 --decode` (add padding `=` if needed).

The decoded result is the flag: `picoCTF{53rv3r_53rv3r_53rv3r_53rv3r_53rv3r}`.

## Tips
- Always check client-side code for hidden secrets in web challenges.
- If stuck, review the challenge hints (though the user mentioned none were visible initially).
- This challenge emphasizes that not all login pages require actual authentication—sometimes the "flag" is in plain sight (or encoded) in the source.

## Flag
`picoCTF{53rv3r_53rv3r_53rv3r_53rv3r_53rv3r}`
