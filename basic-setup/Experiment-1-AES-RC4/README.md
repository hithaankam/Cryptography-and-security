# Experiment 1 — Symmetric Cipher Algorithm (AES and RC4)

## AIM

To implement symmetric key encryption and decryption using AES and RC4 algorithms.

## Tools Required

- Java JDK 8 or later
- Command Prompt / Terminal
- Any Java IDE (optional)

## Concept

Symmetric encryption uses the same secret key for encryption and decryption.

### AES
AES (Advanced Encryption Standard) is a symmetric block cipher. AES commonly uses 128-bit, 192-bit, or 256-bit keys.

### RC4
RC4 is a stream cipher that generates a keystream and combines it with plaintext using XOR. RC4 is insecure for modern applications, but it is useful for demonstrating the algorithm in a cryptography laboratory.

## Procedure

1. Read the plaintext.
2. Use a secret AES key.
3. Encrypt and decrypt the plaintext using AES.
4. Use a secret RC4 key.
5. Encrypt and decrypt the plaintext using RC4.
6. Verify that the decrypted text is the same as the original plaintext.

## Source Code

Save as `SymmetricCipher.java`.

```java
import java.util.*;
import javax.crypto.*;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;
import java.util.Base64;

public class SymmetricCipher {

    static String aesEncrypt(String text, String key) throws Exception {
        SecretKeySpec k = new SecretKeySpec(
                key.getBytes(StandardCharsets.UTF_8), "AES");

        Cipher c = Cipher.getInstance("AES");
        c.init(Cipher.ENCRYPT_MODE, k);

        byte[] encrypted = c.doFinal(
                text.getBytes(StandardCharsets.UTF_8));

        return Base64.getEncoder().encodeToString(encrypted);
    }

    static String aesDecrypt(String text, String key) throws Exception {
        SecretKeySpec k = new SecretKeySpec(
                key.getBytes(StandardCharsets.UTF_8), "AES");

        Cipher c = Cipher.getInstance("AES");
        c.init(Cipher.DECRYPT_MODE, k);

        byte[] decrypted = c.doFinal(
                Base64.getDecoder().decode(text));

        return new String(decrypted, StandardCharsets.UTF_8);
    }

    static byte[] rc4(byte[] data, String key) {
        byte[] s = new byte[256];

        for (int i = 0; i < 256; i++)
            s[i] = (byte) i;

        int j = 0;

        for (int i = 0; i < 256; i++) {
            j = (j + (s[i] & 255) +
                    key.charAt(i % key.length())) % 256;

            byte temp = s[i];
            s[i] = s[j];
            s[j] = temp;
        }

        byte[] result = new byte[data.length];

        int i = 0;
        j = 0;

        for (int n = 0; n < data.length; n++) {
            i = (i + 1) % 256;
            j = (j + (s[i] & 255)) % 256;

            byte temp = s[i];
            s[i] = s[j];
            s[j] = temp;

            int k = s[((s[i] & 255) + (s[j] & 255)) & 255] & 255;

            result[n] = (byte) (data[n] ^ k);
        }

        return result;
    }

    public static void main(String[] args) throws Exception {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter plaintext: ");
        String text = sc.nextLine();

        String aesKey = "1234567890123456";
        String rc4Key = "secret";

        String aesCipher = aesEncrypt(text, aesKey);

        System.out.println("\nAES Encryption:");
        System.out.println("Ciphertext: " + aesCipher);

        System.out.println("AES Decryption:");
        System.out.println("Plaintext: " + aesDecrypt(aesCipher, aesKey));

        byte[] rc4Cipher =
                rc4(text.getBytes(StandardCharsets.UTF_8), rc4Key);

        System.out.println("\nRC4 Encryption:");
        System.out.println("Ciphertext: " +
                Base64.getEncoder().encodeToString(rc4Cipher));

        byte[] rc4Plain = rc4(rc4Cipher, rc4Key);

        System.out.println("RC4 Decryption:");
        System.out.println("Plaintext: " +
                new String(rc4Plain, StandardCharsets.UTF_8));
    }
}
```

## Steps to Execute

### Step 1 — Check Java

```bash
java -version
javac -version
```

### Step 2 — Compile

```bash
javac SymmetricCipher.java
```

### Step 3 — Run

```bash
java SymmetricCipher
```

### Step 4 — Enter Input

Example:

```text
Enter plaintext: Hello World
```

### Step 5 — Verify Output

You should see AES and RC4 ciphertext followed by the original plaintext after decryption.

## Sample Output

```text
Enter plaintext: Hello World

AES Encryption:
Ciphertext: <encrypted-value>
AES Decryption:
Plaintext: Hello World

RC4 Encryption:
Ciphertext: <encrypted-value>
RC4 Decryption:
Plaintext: Hello World
```

The ciphertext is expected to look different from the sample because it is encoded binary data.

## Viva Points

- AES is a symmetric block cipher.
- AES block size is 128 bits.
- AES supports 128, 192, and 256-bit keys.
- RC4 is a stream cipher.
- Symmetric encryption uses the same secret key for encryption and decryption.
