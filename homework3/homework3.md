# CosimoLombardi2031075
## Introduction to RSA Cryptography

The **RSA algorithm** (named after its inventors *Rivest, Shamir, and Adleman*, 1977) is one of the most widely used public-key cryptographic systems. It enables **secure data transmission** and **digital signatures** through the use of asymmetric key pairs — a **public key** for encryption and a **private key** for decryption.

## Theoretical Background

RSA is based on the mathematical properties of **prime numbers** and the difficulty of **integer factorization**. The core idea is that while it is easy to multiply two large primes together, it is computationally infeasible to determine those primes given only their product.

### Key Concepts

1. **Key Generation**
   - Two large prime numbers `p` and `q` are chosen.
   - Their product `n = p × q` forms part of both the public and private keys.
   - The **Euler’s totient function** is calculated as:

     ```
     φ(n) = (p - 1)(q - 1)
     ```

   - A public exponent `e` is selected such that `1 < e < φ(n)` and `gcd(e, φ(n)) = 1`.
   - The private exponent `d` is computed as the modular inverse of `e` modulo `φ(n)`:

     ```
     d × e ≡ 1 (mod φ(n))
     ```

2. **Encryption**
   - A plaintext message `M` is converted into an integer `m` such that `0 < m < n`.
   - The ciphertext `c` is computed as:

     ```
     c ≡ m^e (mod n)
     ```

3. **Decryption**
   - The original message is recovered using the private key:

     ```
     m ≡ c^d (mod n)
     ```

## Security Considerations

The security of RSA relies on the **computational hardness** of factoring large composite numbers. While modern computers can factor small numbers efficiently, factoring a 2048-bit RSA modulus is currently beyond feasible computational limits. However, potential advances in **quantum computing** (e.g., *Shor’s algorithm*) pose a future threat to RSA’s security.

