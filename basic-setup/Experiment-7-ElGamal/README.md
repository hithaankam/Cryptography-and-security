# Experiment 7 — ElGamal Cryptosystem

## AIM

To implement the ElGamal public-key cryptosystem for encryption and decryption of a message.

## Tools Required

- Java JDK 8 or later
- Command Prompt / Terminal
- Java IDE (optional)

## Concept

ElGamal is an asymmetric public-key cryptosystem based on the discrete logarithm problem.

Choose:

```text
p = prime number
g = generator
x = private key
```

Public key:

```text
y = g^x mod p
```

For encryption, select random `k`.

```text
c1 = g^k mod p
c2 = m * y^k mod p
```

Ciphertext is:

```text
(c1, c2)
```

For decryption:

```text
s = c1^x mod p
m = c2 * s^-1 mod p
```

where `s^-1` is the modular inverse of `s`.

## Procedure

1. Select prime `p`.
2. Select generator `g`.
3. Select private key `x`.
4. Calculate public key `y`.
5. Enter message `m`, where `m < p`.
6. Select random value `k`.
7. Calculate `c1`.
8. Calculate `c2`.
9. Calculate the shared value during decryption.
10. Find its modular inverse.
11. Recover the original message.

## Source Code

Save as `ElGamal.java`.

```java
import java.util.*;

public class ElGamal {

    static long power(long a, long b, long p) {

        long result = 1;

        for (int i = 0; i < b; i++)
            result = (result * a) % p;

        return result;
    }

    static long inverse(long a, long p) {

        for (long i = 1; i < p; i++) {
            if ((a * i) % p == 1)
                return i;
        }

        return -1;
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter prime p: ");
        long p = sc.nextLong();

        System.out.print("Enter generator g: ");
        long g = sc.nextLong();

        System.out.print("Enter private key x: ");
        long x = sc.nextLong();

        System.out.print("Enter message m (< p): ");
        long m = sc.nextLong();

        System.out.print("Enter random k: ");
        long k = sc.nextLong();

        long y = power(g, x, p);

        long c1 = power(g, k, p);

        long s = power(y, k, p);

        long c2 = (m * s) % p;

        System.out.println(
                "\nPublic Key: (" + p + ", " + g + ", " + y + ")");

        System.out.println(
                "Ciphertext: (" + c1 + ", " + c2 + ")");

        long secret = power(c1, x, p);

        long inv = inverse(secret, p);

        long decrypted = (c2 * inv) % p;

        System.out.println(
                "Decrypted Message: " + decrypted);
    }
}
```

## Steps to Execute

### Step 1 — Compile

```bash
javac ElGamal.java
```

### Step 2 — Run

```bash
java ElGamal
```

### Step 3 — Enter Example Input

```text
Enter prime p: 23
Enter generator g: 5
Enter private key x: 6
Enter message m (< p): 10
Enter random k: 3
```

### Step 4 — Observe Output

```text
Public Key: (23, 5, 8)
Ciphertext: (10, 8)
Decrypted Message: 10
```

The ciphertext depends on the selected `k`.

## Viva Points

- ElGamal is an asymmetric encryption algorithm.
- It uses a public key and private key.
- Its security is based on the discrete logarithm problem.
- Encryption uses a fresh random value `k`.
- The ciphertext consists of two values.
- The message must satisfy `m < p` in this simple integer implementation.
