# Experiment 13 — Message Authentication Codes

## Aim
To implement a Message Authentication Code using HMAC-SHA256 in Java.

## Requirements
- Java JDK 8 or later
- VS Code / IntelliJ IDEA / Eclipse
- Terminal or Command Prompt

## Concept
A Message Authentication Code (MAC) provides:
- Message authentication
- Message integrity

The sender and receiver share a secret key.

The sender calculates:

`MAC = HMAC(secret key, message)`

The receiver calculates the MAC again using the same key.

If the two MAC values match, the message is considered authentic and unchanged.

HMAC-SHA256 is a common hash-based MAC construction.

A MAC does not provide confidentiality because the message itself is not encrypted.

## Procedure
1. Create a message.
2. Generate a secret HMAC key.
3. Initialize HMAC-SHA256.
4. Generate the MAC.
5. Display the message and MAC.
6. Recalculate the MAC at the receiver.
7. Compare both MAC values.
8. Display the verification result.

## Source Code

Save as `MACDemo.java`:

```java
import javax.crypto.KeyGenerator;
import javax.crypto.Mac;
import javax.crypto.SecretKey;
import java.util.Base64;

public class MACDemo {

    public static void main(String[] args)
            throws Exception {

        String message =
                "This is a secure message.";

        KeyGenerator keyGenerator =
                KeyGenerator.getInstance("HmacSHA256");

        SecretKey key =
                keyGenerator.generateKey();

        Mac mac =
                Mac.getInstance("HmacSHA256");

        mac.init(key);

        byte[] generatedMAC =
                mac.doFinal(message.getBytes());

        String macValue =
                Base64.getEncoder()
                        .encodeToString(generatedMAC);

        System.out.println("Message:");
        System.out.println(message);

        System.out.println("\nMAC:");
        System.out.println(macValue);

        mac.init(key);

        byte[] receivedMAC =
                mac.doFinal(message.getBytes());

        boolean verified =
                java.util.Arrays.equals(
                        generatedMAC,
                        receivedMAC
                );

        System.out.println(
                "\nMAC Verification: " + verified
        );
    }
}
```

## How to Execute

```bash
javac MACDemo.java
java MACDemo
```

## Sample Output

```text
Message:
This is a secure message.

MAC:
...

MAC Verification: true
```

## Demonstrating Tampering

Change the message before the receiver calculates the MAC:

```java
String message =
        "This is a modified message.";
```

The calculated MAC will no longer match the original MAC when a real sender/receiver flow is simulated.

Expected verification result:

```text
MAC Verification: false
```

## Result
HMAC-SHA256 was successfully implemented and the integrity and authenticity of the message were verified.

## Important Exam Points
- MAC provides authentication and integrity.
- MAC does not provide confidentiality.
- HMAC stands for Hash-based Message Authentication Code.
- HMAC-SHA256 uses SHA-256 inside the HMAC construction.
- Sender and receiver must share a secret key.
