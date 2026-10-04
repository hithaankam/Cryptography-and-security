# Experiment 11 — Trusted Secure Web Transaction

## Aim
To implement a basic trusted secure web transaction using encryption in Java.

## Requirements
- Java JDK 8 or later
- VS Code / IntelliJ IDEA / Eclipse
- Terminal or Command Prompt

## Concept
A secure web transaction should provide confidentiality, integrity, authentication, and, where appropriate, non-repudiation.

This experiment demonstrates confidentiality using AES encryption and verifies that the decrypted transaction matches the original transaction.

Flow:

```text
Transaction
    |
    v
AES Encryption
    |
    v
Ciphertext
    |
    v
AES Decryption
    |
    v
Original Transaction
```

This is a small educational simulation rather than a complete HTTPS/TLS implementation.

## Procedure
1. Create transaction information.
2. Generate an AES key.
3. Encrypt the transaction.
4. Display the encrypted transaction.
5. Decrypt it using the same AES key.
6. Compare the decrypted transaction with the original.
7. Display successful verification.

## Source Code

Save as `SecureWebTransaction.java`:

```java
import javax.crypto.Cipher;
import javax.crypto.KeyGenerator;
import javax.crypto.SecretKey;
import java.util.Base64;

public class SecureWebTransaction {

    public static void main(String[] args) throws Exception {

        String transaction =
                "User=Hitha; Amount=5000; Account=123456";

        KeyGenerator keyGenerator =
                KeyGenerator.getInstance("AES");

        keyGenerator.init(128);

        SecretKey key =
                keyGenerator.generateKey();

        Cipher cipher =
                Cipher.getInstance("AES");

        cipher.init(Cipher.ENCRYPT_MODE, key);

        byte[] encrypted =
                cipher.doFinal(transaction.getBytes());

        String encoded =
                Base64.getEncoder()
                        .encodeToString(encrypted);

        System.out.println("Original Transaction:");
        System.out.println(transaction);

        System.out.println("\nEncrypted Transaction:");
        System.out.println(encoded);

        cipher.init(Cipher.DECRYPT_MODE, key);

        byte[] decrypted =
                cipher.doFinal(encrypted);

        String result =
                new String(decrypted);

        System.out.println("\nDecrypted Transaction:");
        System.out.println(result);

        if (transaction.equals(result)) {
            System.out.println(
                "\nTransaction verified successfully."
            );
        }
    }
}
```

## How to Execute

```bash
javac SecureWebTransaction.java
java SecureWebTransaction
```

## Sample Output

```text
Original Transaction:
User=Hitha; Amount=5000; Account=123456

Encrypted Transaction:
...

Decrypted Transaction:
User=Hitha; Amount=5000; Account=123456

Transaction verified successfully.
```

The encrypted value changes because a new AES key is generated each time.

## Result
The secure transaction was encrypted and successfully recovered through decryption, demonstrating confidentiality.

## Important Exam Points
- AES is symmetric encryption.
- The same secret key is used for encryption and decryption.
- Confidentiality prevents unauthorized reading.
- A real web transaction normally uses TLS/HTTPS and additional authentication/integrity mechanisms.
