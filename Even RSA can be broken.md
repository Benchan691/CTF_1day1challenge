# RSA Decryption Summary

## Problem Setup
- **Modulus (N)**: 22673355849281161824225271070384824415231707045191507016563305021624252406385518508580129898315638646907373306899439095393409336559799395764762450691637898
- **Public Exponent (e)**: 65537
- **Ciphertext (c)**: 6384157316558928066175098590632582063897566761287440430528071378951896753949142044805688385393913000648279177620634758225976984094835389615648458651390291

The task was to find the private exponent \(d\) and decrypt the ciphertext to recover the plaintext message \(m\).

## Steps to Find \(d\)
1. **Factorize N**:  
   Notice that \(N\) is even, so one prime factor is \(p = 2\). The other factor is \(q = N // 2\).

2. **Compute Euler's Totient \(\phi(N)\)**:  
   \(\phi(N) = (p - 1) \times (q - 1)\).

3. **Compute Private Exponent \(d\)**:  
   \(d\) is the modular inverse of \(e\) modulo \(\phi(N)\), calculated as \(d = e^{-1} \mod \phi(N)\).

## Steps to Find Plaintext \(m\)
1. **Decrypt Ciphertext**:  
   Compute \(m_{\text{int}} = c^d \mod N\).

2. **Convert to String**:  
   Convert the integer \(m_{\text{int}}\) to hexadecimal, then to bytes, and decode as UTF-8 to get the plaintext string.

## Result
- **Plaintext Message**: picoCTF{tw0_1$_pr!m302ce519d}

This appears to be a flag from a Capture The Flag (CTF) challenge, hinting at the factorization where 2 is a prime factor.

## Python Code Snippet
```python
N = 22673355849281161824225271070384824415231707045191507016563305021624252406385518508580129898315638646907373306899439095393409336559799395764762450691637898
e = 65537
c = 6384157316558928066175098590632582063897566761287440430528071378951896753949142044805688385393913000648279177620634758225976984094835389615648458651390291

p = 2
q = N // 2
phi = (p - 1) * (q - 1)
d = pow(e, -1, phi)
m_int = pow(c, d, N)
m_hex = hex(m_int)[2:]
m_bytes = bytes.fromhex(m_hex)
m_str = m_bytes.decode('utf-8')

print(m_str)  # Output: picoCTF{tw0_1$_pr!m302ce519d}
```
