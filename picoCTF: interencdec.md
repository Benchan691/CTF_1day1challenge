# interencdec PicoCTF Writeup

## Challenge Description
interencdec is a cryptography challenge from picoCTF 2024. It involves decoding a multi-layered encoded message to reveal the flag. The provided hint is: "Engaging in various decoding processes is of utmost importance." The encoded secret message is: `YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclgya3lNRFJvYTJvMmZRPT0nCg==`.

## Steps to Solve

1. **Identify the Encoding**: Use a cipher identification tool (e.g., dcode.fr/cipher-identifier) to analyze the message. It recognizes the string as Base64-encoded due to the characteristic alphabet (A-Z, a-z, 0-9, +, /) and padding (=).

2. **First Base64 Decode**:
   - Decode the message using a Base64 decoder.
   - If using Python (e.g., `import base64; base64.b64decode('YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclgya3lNRFJvYTJvMmZRPT0nCg==')`), the output will be bytes: `b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2kyMDRoa2o2fQ==\n'`.
   - **Mention of the b' Issue**: In Python, `base64.b64decode()` returns a bytes object (prefixed with `b'` when printed), which includes any trailing newline (`\n`) from the original encoding. This can cause issues if not handled—strip whitespace and convert to a string before the next decode: `decoded1 = base64.b64decode(original).strip().decode('ascii')`, resulting in `"d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2kyMDRoa2o2fQ=="`. Attempting to decode the bytes directly (including the `b'` or quotes) will fail with errors like "Invalid base64-encoded string".

3. **Second Base64 Decode**:
   - Decode the cleaned result from Step 2: `"d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2kyMDRoa2o2fQ=="`.
   - This yields: `"wpjvJAM{jhlzhy_k3jy9wa3k_i204hkj6}"`.
   - The `{...}` structure suggests this is close to a picoCTF flag but still encrypted.

4. **Identify and Decrypt the Inner Cipher**:
   - Analyze the string with a cipher tool—it identifies as a Caesar cipher (ROT/shift cipher), a substitution method shifting letters by a fixed number (preserving case and non-letters).
   - Test shifts (0-25) or use a brute-force tool. A shift of 7 (or equivalently ROT-19, since 26-7=19) decrypts it:
     - For each letter: Subtract 7 positions (mod 26).
     - Example: 'w' (22) - 7 = 15 → 'p'; 'p' (15) - 7 = 8 → 'i'; etc.
   - Decrypted result: `picoCTF{caesar_d3cr9pt3d_b204adc6}`.

5. **Submit the Flag**: Enter `picoCTF{caesar_d3cr9pt3d_b204adc6}` in the picoCTF portal.

## Tools Used
- Cipher identifier (e.g., dcode.fr)
- Base64 decoder (online or Python's base64 module)
- Caesar/ROT decoder (online brute-forcer or custom script)

This challenge demonstrates layered encodings (double Base64) combined with simple classical encryption (Caesar cipher).
