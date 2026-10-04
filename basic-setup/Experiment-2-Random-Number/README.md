# Experiment 2 — Random Number Generation Using Digits and Alphabets

## AIM

To generate a random string using a specified subset of alphabets and digits.

## Tools Required

- Java JDK 8 or later
- Command Prompt / Terminal
- Java IDE (optional)

## Concept

A character set can contain uppercase letters, lowercase letters, and digits.

Example:

```text
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789
```

A random index is generated for each required character, and the character at that index is selected.

## Procedure

1. Define the allowed characters.
2. Read the required length.
3. Generate a random index.
4. Select the character at that index.
5. Repeat until the required length is reached.
6. Display the generated random string.

## Source Code

Save as `RandomString.java`.

```java
import java.util.*;

public class RandomString {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        String chars =
                "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

        System.out.print("Enter length: ");
        int n = sc.nextInt();

        Random r = new Random();

        String result = "";

        for (int i = 0; i < n; i++) {
            int index = r.nextInt(chars.length());
            result += chars.charAt(index);
        }

        System.out.println("Random String: " + result);
    }
}
```

## Steps to Execute

### Step 1

Open Command Prompt or Terminal.

### Step 2

Go to the directory containing `RandomString.java`.

### Step 3 — Compile

```bash
javac RandomString.java
```

### Step 4 — Run

```bash
java RandomString
```

### Step 5 — Enter Length

For example:

```text
Enter length: 10
```

### Step 6 — Observe Output

```text
Random String: aG7kP2mX91
```

Your output will normally be different.

## Viva Points

- `Random` is used to generate pseudorandom values.
- `nextInt(n)` generates a value from 0 to `n-1`.
- The character set determines which characters can appear.
- Random strings can be used for demonstrations of tokens, identifiers, and test data.
