# Experiment 6 — Diffie-Hellman Algorithm

## AIM

To implement the Diffie-Hellman key exchange algorithm for establishing a shared secret key between two parties.

## Tools Required

- Java JDK 8 or later
- Command Prompt / Terminal
- Java IDE (optional)

## Concept

Diffie-Hellman allows two parties to establish a common secret without directly transmitting the secret.

Let `p` be a prime and `g` be a primitive root.

Alice chooses private key `a`:

```text
A = g^a mod p
```

Bob chooses private key `b`:

```text
B = g^b mod p
```

Shared secret:

```text
Alice: K = B^a mod p
Bob:   K = A^b mod p
```

Both values are equal:

```text
K = g^(ab) mod p
```

## Procedure

1. Select prime number `p`.
2. Select primitive root `g`.
3. Select Alice's private key `a`.
4. Select Bob's private key `b`.
5. Calculate Alice's public key.
6. Calculate Bob's public key.
7. Calculate both shared secrets.
8. Verify that the two shared secrets are equal.

## Source Code

Save as `DiffieHellman.java`.

```java
import java.util.*;

public class DiffieHellman {

    static long power(long a, long b, long p) {

        long result = 1;

        for (int i = 0; i < b; i++)
            result = (result * a) % p;

        return result;
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter prime number p: ");
        long p = sc.nextLong();

        System.out.print("Enter primitive root g: ");
        long g = sc.nextLong();

        System.out.print("Enter Alice private key: ");
        long a = sc.nextLong();

        System.out.print("Enter Bob private key: ");
        long b = sc.nextLong();

        long A = power(g, a, p);
        long B = power(g, b, p);

        long key1 = power(B, a, p);
        long key2 = power(A, b, p);

        System.out.println("Alice Public Key: " + A);
        System.out.println("Bob Public Key: " + B);

        System.out.println("Alice Shared Secret: " + key1);
        System.out.println("Bob Shared Secret: " + key2);

        if (key1 == key2)
            System.out.println(
                    "Key Exchange Successful.");
        else
            System.out.println(
                    "Key Exchange Failed.");
    }
}
```

## Steps to Execute

### Step 1 — Compile

```bash
javac DiffieHellman.java
```

### Step 2 — Run

```bash
java DiffieHellman
```

### Step 3 — Enter Example Input

```text
Enter prime number p: 23
Enter primitive root g: 5
Enter Alice private key: 6
Enter Bob private key: 15
```

### Step 4 — Observe Output

```text
Alice Public Key: 8
Bob Public Key: 19
Alice Shared Secret: 2
Bob Shared Secret: 2
Key Exchange Successful.
```

## Viva Points

- Diffie-Hellman is a key-exchange algorithm.
- It does not directly encrypt the message.
- Both parties obtain the same shared secret.
- Security is based on the difficulty of the discrete logarithm problem.
- Private keys must be kept secret.
