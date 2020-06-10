# Glossary

## Fallback / Failover / Failback
- fallback: 可以是名詞或形容詞，替代或後備的意思
- failover: 主要方案失敗，移轉到後備方案的過程  
   *primary error   ---(failover)-->   secondary (fallback)*
- failback: 主要方案又變成可行了，從後備方案回復到主要方案的過程  
   *secondary (fallback)   ---(failback)-->   primary recovered*

## Symmetric vs. Asymmetric Encryption
- Symmetric: Use the same key for encryption and decryption. (e.g. AES)
   - You have to share the key before encrypt / decrypt. (vulnerable to being intercepted, man-in-the-middle attacks)
- Asymmetric: Use a pair of keys for encryption and decryption. (e.g. RSA)
   - A public key is made freely available to anyone who want to send you a message.
   - A private key is kept a serect so that only you can know.
   - Messages encrypted using a public key can only be decrypted using a private key, and vice versa.  
   (encrypted using a private key can only be decrypted using a public key).
   - Still vulnerable to man-in-the-middle attacks, but easier. (Solution: [PKI](https://en.wikipedia.org/wiki/Public_key_infrastructure))