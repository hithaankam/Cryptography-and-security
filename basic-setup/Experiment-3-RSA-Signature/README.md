# Experiment 3 — RSA Based Signature System

## AIM

To implement a digital signature system using RSA.

## Tools Required

- Java JDK 8 or later
- Command Prompt / Terminal
- Java IDE (optional)

## Concept

RSA is an asymmetric cryptographic algorithm that uses a public key and a private key.

For a digital signature:

```text
Private Key -> Sign
Public Key  -> Verify
```

A signature helps provide integrity, authentication, and non-repudiation.

This program uses Java's `SHA256withRSA` signature implementation.

## Procedure

1. Generate an RSA key pair.
2. Read a message.
3. Sign the message using the private key.
4. Display the signature.
5. Verify the signature using the public key.
6. Display whether verification succeeded.

## Source Code

Save as `RSASignature.java`.

```java
import java.security.*;
import java.util.*;

public class RSASignature {

    public static void main(String[] args) throws Exception {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter message: ");
        String message = sc.nextLine();

        KeyPairGenerator kpg =
                KeyPairGenerator.getInstance("RSA");

        kpg.initialize(2048);

        KeyPair kp = kpg.generateKeyPair();

        PrivateKey privateKey = kp.getPrivate();
        PublicKey publicKey = kp.getPublic();

        Signature sign =
                Signature.getInstance("SHA256withRSA");

        sign.initSign(privateKey);
        sign.update(message.getBytes());

        byte[] signature = sign.sign();

        System.out.println("Digital Signature:");
        System.out.println(
                Base64.getEncoder().encodeToString(signature));

        Signature verify =
                Signature.getInstance("SHA256withRSA");

        verify.initVerify(publicKey);
        verify.update(message.getBytes());

        boolean valid = verify.verify(signature);

        System.out.println("Signature Valid: " + valid);
    }
}
```

## Steps to Execute

### Step 1 — Compile

```bash
javac RSASignature.java
```

### Step 2 — Run

```bash
java RSASignature
```

### Step 3 — Enter Message

```text
Enter message: Hello RSA
```

### Step 4 — Observe Output

The program generates an RSA key pair, signs the message, and verifies the signature.

## Sample Output

```text
Enter message: Hello RSA
Digital Signature:
<signature-value>
Signature Valid: true
```

The signature value will be different for different generated keys.

## Viva Points

- RSA is an asymmetric algorithm.
- The private key is kept secret.
- The public key can be shared.
- A private key is used to create an RSA signature.
- The corresponding public key verifies it.
- The program uses RSA with SHA-256 for the signature operation.
