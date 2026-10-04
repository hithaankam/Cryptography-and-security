# Experiment 5 — Authenticating a Given Signature Using MD5

## AIM

To generate an MD5 hash for a message and authenticate a given signature/hash by comparing it with the generated MD5 hash.

## Tools Required

- Java JDK 8 or later
- Command Prompt / Terminal
- Java IDE (optional)

## Concept

MD5 is a cryptographic hash function that produces a 128-bit hash.

```text
Message
   |
   v
  MD5
   |
   v
128-bit Hash
```

If the message is changed, the resulting hash changes.

**Security note:** MD5 is cryptographically broken and should not be used for modern security applications. It is used here because the laboratory experiment specifically asks for MD5.

## Procedure

1. Read the original message.
2. Generate its MD5 hash.
3. Read the supplied hash/signature.
4. Compare the two hash values.
5. If they match, authenticate the signature.
6. Otherwise, report authentication failure.

## Source Code

Save as `MD5Authentication.java`.

```java
import java.security.*;
import java.util.*;

public class MD5Authentication {

    static String md5(String text) throws Exception {

        MessageDigest md =
                MessageDigest.getInstance("MD5");

        byte[] hash = md.digest(text.getBytes());

        StringBuilder sb = new StringBuilder();

        for (byte b : hash)
            sb.append(String.format("%02x", b));

        return sb.toString();
    }

    public static void main(String[] args) throws Exception {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter message: ");
        String message = sc.nextLine();

        String generatedHash = md5(message);

        System.out.println(
                "Generated MD5 Hash: " + generatedHash);

        System.out.print(
                "Enter given signature/hash: ");

        String givenHash = sc.nextLine();

        if (generatedHash.equalsIgnoreCase(givenHash))
            System.out.println(
                    "Signature Authenticated.");
        else
            System.out.println(
                    "Signature Authentication Failed.");
    }
}
```

## Steps to Execute

### Step 1 — Compile

```bash
javac MD5Authentication.java
```

### Step 2 — Run

```bash
java MD5Authentication
```

### Step 3 — Enter Message

For example:

```text
Enter message: Hello
```

The program displays:

```text
Generated MD5 Hash: 8b1a9953c4611296a827abf8c47804d7
```

### Step 4 — Enter the Same Hash

```text
Enter given signature/hash: 8b1a9953c4611296a827abf8c47804d7
```

### Step 5 — Observe

```text
Signature Authenticated.
```

If you enter an incorrect hash:

```text
Signature Authentication Failed.
```

## Viva Points

- MD5 produces a 128-bit hash.
- Hashing is a one-way operation in normal use.
- A small input change produces a different hash.
- MD5 is no longer considered secure for cryptographic authentication.
