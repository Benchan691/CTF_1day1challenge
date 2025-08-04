# WebDecode PicoCTF Writeup

## Challenge Description
WebDecode is an easy web exploitation challenge from picoCTF 2024. The goal is to find and decode a hidden flag within the website's source code. The website consists of three pages: Home (index.html), About (about.html), and Contact (contact.html), along with a CSS style file (style.css).

## Steps to Solve

1. **Access the Website**: Open the provided challenge URL in your browser. You'll see a simple website with navigation links to Home, About, and Contact.

2. **Explore the Pages**:
   - **Home (index.html)**: This page doesn't contain any useful information for the flag.
   - **Contact (contact.html)**: Displays a message like "Keep searching page. Don’t give up!!!" but no flag.
   - **About (about.html)**: Displays a hint: "Try inspecting the page!! You might find it there."

3. **Inspect the About Page**:
   - Right-click on the About page and select "Inspect" (or press Ctrl+Shift+I) to open the browser's developer tools.
   - Go to the "Elements" tab and examine the HTML source code.
   - Look for the `<section>` tag with the class "about". You'll find an attribute called `notify_true` containing a Base64-encoded string:
     ```
     <section class="about" notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfZjZmNmI3OGF9">
     ```

4. **Decode the Base64 String**:
   - Copy the encoded value: `cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfZjZmNmI3OGF9`.
   - Use a Base64 decoder tool (e.g., CyberChef, online decoder, or command line):
     - Command line example: `echo "cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfZjZmNmI3OGF9" | base64 -d`
     - Python example:
       ```python
       import base64
       encoded = "cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfZjZmNmI3OGF9"
       decoded = base64.b64decode(encoded).decode('utf-8')
       print(decoded)
       ```
   - The decoded string is the flag: `picoCTF{web_succ3ssfully_d3c0ded_f6f6b78a}`.

5. **Submit the Flag**: Enter the flag in the picoCTF submission portal to complete the challenge.

## Tools Used
- Browser Developer Tools (Inspector)
- Base64 Decoder (e.g., online tool or Python script)

This challenge teaches basic web inspection techniques and decoding common encodings like Base64.
<argument name="citation_id">15</argument>

<argument name="citation_id">16</argument>
