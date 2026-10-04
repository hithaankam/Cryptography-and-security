# Experiment 12 — Digital Certificates, Hybrid Encryption and PKI

## Aim
To demonstrate hybrid encryption using RSA and AES and understand the role of digital certificates and PKI.

## Requirements
- Java JDK 8 or later
- VS Code / IntelliJ IDEA / Eclipse
- Terminal or Command Prompt

## Concept
Hybrid encryption combines symmetric and asymmetric cryptography.

AES is used to encrypt the actual data because it is efficient.

RSA is used to protect the AES session key.

Flow:

```text
                 AES
Message --------------------> Encrypted Message

AES Key
   |
   v
RSA Public Key
   |
   v
Encrypted AES Key
```

The receiver uses the RSA private key to recover the AES key and then uses AES to decrypt the message.

PKI (Public Key Infrastructure) manages public keys, private keys, certificates, and Certificate Authorities.

A digital certificate binds an identity to a public key and is normally signed by a trusted Certificate Authority.

The following program demonstrates the hybrid-encryption part. A full certificate authority is not required for this small lab demonstration.

## Procedure
1. Generate an RSA key pair.
2. Generate an AES session key.
3. Encrypt the message using AES.
4. Encrypt the AES key using the RSA public key.
5. Use the RSA private key to recover the AES key.
6. Use the recovered AES key to decrypt the message.
7. Verify that the original message is recovered.

## Source Code

Save as `HybridEncryption.java`:

```java
import javax.crypto.Cipher;
import javax.crypto.KeyGenerator;
import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import java.security.KeyPair;
import java.security.KeyPairGenerator;
import java.util.Base64;

public class HybridEncryption {

    public static void main(String[] args) throws Exception {

        String message =
                "This is a confidential message.";

        KeyPairGenerator rsaGenerator =
                KeyPairGenerator.getInstance("RSA");

        rsaGenerator.initialize(2048);

        KeyPair rsaKeys =
                rsaGenerator.generateKeyPair();

        KeyGenerator aesGenerator =
                KeyGenerator.getInstance("AES");

        aesGenerator.init(128);

        SecretKey aesKey =
                aesGenerator.generateKey();

        Cipher aes =
                Cipher.getInstance("AES");

        aes.init(Cipher.ENCRYPT_MODE, aesKey);

        byte[] encryptedMessage =
                aes.doFinal(message.getBytes());

        Cipher rsa =
                Cipher.getInstance("RSA");

        rsa.init(
                Cipher.ENCRYPT_MODE,
                rsaKeys.getPublic()
        );

        byte[] encryptedAESKey =
                rsa.doFinal(aesKey.getEncoded());

        System.out.println("Original Message:");
        System.out.println(message);

        System.out.println("\nEncrypted AES Key:");
        System.out.println(
                Base64.getEncoder()
                        .encodeToString(encryptedAESKey)
        );

        System.out.println("\nEncrypted Message:");
        System.out.println(
                Base64.getEncoder()
                        .encodeToString(encryptedMessage)
        );

        rsa.init(
                Cipher.DECRYPT_MODE,
                rsaKeys.getPrivate()
        );

        byte[] decryptedAESKey =
                rsa.doFinal(encryptedAESKey);

        SecretKey recoveredKey =
                new SecretKeySpec(
                        decryptedAESKey,
                        "AES"
                );

        aes.init(
                Cipher.DECRYPT_MODE,
                recoveredKey
        );

        byte[] decryptedMessage =
                aes.doFinal(encryptedMessage);

        System.out.println("\nDecrypted Message:");
        System.out.println(
                new String(decryptedMessage)
        );
    }
}
```

## How to Execute

```bash
javac HybridEncryption.java
java HybridEncryption
```

## Sample Output

```text
Original Message:
This is a confidential message.

Encrypted AES Key:
...

Encrypted Message:
...

Decrypted Message:
This is a confidential message.
```

## Result
Hybrid encryption was successfully demonstrated by encrypting the message using AES and protecting the AES session key using RSA.

## Important Exam Points
- AES = symmetric encryption.
- RSA = asymmetric encryption.
- Hybrid encryption uses both.
- PKI = Public Key Infrastructure.
- CA = Certificate Authority.
- Digital certificate binds an identity to a public key.
- RSA is not normally used to encrypt large application data directly.
