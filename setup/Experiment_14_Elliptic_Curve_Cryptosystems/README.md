# Experiment 14 — Elliptic Curve Cryptosystems

## Aim
To implement an Elliptic Curve Cryptosystem concept using ECDH key agreement in Java.

## Requirements
- Java JDK 8 or later
- VS Code / IntelliJ IDEA / Eclipse
- Terminal or Command Prompt

## Concept
Elliptic Curve Cryptography (ECC) is based on mathematical operations on elliptic curves.

A simplified elliptic curve can be represented as:

`y² = x³ + ax + b`

ECC security is related to the difficulty of the Elliptic Curve Discrete Logarithm Problem (ECDLP).

ECDH (Elliptic Curve Diffie-Hellman) allows two parties to establish a common shared secret.

Flow:

```text
Alice Private Key + Bob Public Key
              |
              v
       Alice Shared Secret

Bob Private Key + Alice Public Key
              |
              v
        Bob Shared Secret
```

Both shared secrets should be equal.

## Procedure
1. Generate an EC key pair for Alice.
2. Generate an EC key pair for Bob.
3. Alice uses her private key and Bob's public key.
4. Bob uses his private key and Alice's public key.
5. Both generate a shared secret.
6. Compare the shared secrets.
7. If they match, ECDH key agreement is successful.

## Source Code

Save as `ECCDemo.java`:

```java
import javax.crypto.KeyAgreement;
import java.security.KeyPair;
import java.security.KeyPairGenerator;
import java.security.MessageDigest;
import java.util.Arrays;

public class ECCDemo {

    public static void main(String[] args)
            throws Exception {

        KeyPairGenerator keyGenerator =
                KeyPairGenerator.getInstance("EC");

        keyGenerator.initialize(256);

        KeyPair alice =
                keyGenerator.generateKeyPair();

        KeyPair bob =
                keyGenerator.generateKeyPair();

        KeyAgreement aliceAgreement =
                KeyAgreement.getInstance("ECDH");

        aliceAgreement.init(alice.getPrivate());

        aliceAgreement.doPhase(
                bob.getPublic(),
                true
        );

        byte[] aliceSecret =
                aliceAgreement.generateSecret();

        KeyAgreement bobAgreement =
                KeyAgreement.getInstance("ECDH");

        bobAgreement.init(bob.getPrivate());

        bobAgreement.doPhase(
                alice.getPublic(),
                true
        );

        byte[] bobSecret =
                bobAgreement.generateSecret();

        System.out.println(
                "Alice Shared Secret: "
                + Arrays.toString(aliceSecret)
        );

        System.out.println(
                "Bob Shared Secret: "
                + Arrays.toString(bobSecret)
        );

        System.out.println(
                "\nShared Secret Equal: "
                + Arrays.equals(
                        aliceSecret,
                        bobSecret
                )
        );

        MessageDigest sha256 =
                MessageDigest.getInstance("SHA-256");

        byte[] derivedKey =
                sha256.digest(aliceSecret);

        System.out.println(
                "\nDerived Key Length: "
                + derivedKey.length * 8
                + " bits"
        );
    }
}
```

## How to Execute

```bash
javac ECCDemo.java
java ECCDemo
```

## Sample Output

```text
Alice Shared Secret: [...]
Bob Shared Secret: [...]

Shared Secret Equal: true

Derived Key Length: 256 bits
```

The actual secret bytes will be different on every execution.

## Result
ECDH key agreement was successfully implemented. Alice and Bob generated the same shared secret without directly transmitting the secret.

## Important Exam Points
- ECC = Elliptic Curve Cryptography.
- ECDH = Elliptic Curve Diffie-Hellman.
- ECC is based on the elliptic curve discrete logarithm problem.
- ECC provides strong security with relatively small key sizes.
- ECDH establishes a shared secret; it is not itself a bulk data encryption algorithm.
