# Experiment 8 — Goldwasser-Micali Probabilistic Public Key System

## Aim
To implement the Goldwasser-Micali probabilistic public key encryption system using Java.

## Requirements
- Java JDK 8 or later
- VS Code / IntelliJ IDEA / Eclipse
- Terminal or Command Prompt

## Concept
Goldwasser-Micali (GM) is a probabilistic public-key encryption system that encrypts one bit at a time.

Choose two primes `p` and `q` and calculate:

`n = p × q`

The public key is `(n, y)` and the private key is `(p, q)`.

For plaintext bit `m`:

- If `m = 0`: `c = r² mod n`
- If `m = 1`: `c = y × r² mod n`

A random value `r` is used, so encrypting the same bit more than once can produce different ciphertexts.

## Procedure
1. Select prime numbers `p` and `q`.
2. Calculate `n = p × q`.
3. Select a suitable quadratic non-residue `y`.
4. Use `(n, y)` as the public key.
5. Use `(p, q)` as the private key.
6. Encrypt every plaintext bit using a random `r`.
7. Decrypt each ciphertext using the private key.
8. Compare the original and decrypted bit strings.

## Source Code

Save as `GoldwasserMicali.java`:

```java
import java.math.BigInteger;
import java.util.Random;

public class GoldwasserMicali {

    static BigInteger p = BigInteger.valueOf(11);
    static BigInteger q = BigInteger.valueOf(19);
    static BigInteger n = p.multiply(q);
    static BigInteger y = BigInteger.valueOf(17);
    static Random random = new Random();

    static BigInteger encrypt(int m) {
        BigInteger r;

        do {
            r = new BigInteger(n.bitLength(), random);
        } while (r.compareTo(BigInteger.ONE) < 0 ||
                 r.compareTo(n) >= 0 ||
                 !r.gcd(n).equals(BigInteger.ONE));

        BigInteger rSquare = r.multiply(r).mod(n);

        if (m == 0) {
            return rSquare;
        } else {
            return y.multiply(rSquare).mod(n);
        }
    }

    static int decrypt(BigInteger c) {
        BigInteger cp = c.mod(p);

        BigInteger result =
                cp.modPow(
                    p.subtract(BigInteger.ONE)
                     .divide(BigInteger.TWO),
                    p
                );

        if (result.equals(BigInteger.ONE)) {
            return 0;
        }

        return 1;
    }

    public static void main(String[] args) {

        int[] message = {1, 0, 1, 1, 0};

        System.out.println("Goldwasser-Micali Cryptosystem");
        System.out.println("Public Key  : (" + n + ", " + y + ")");
        System.out.println("Private Key : (" + p + ", " + q + ")");

        System.out.print("Original Message : ");
        for (int bit : message) {
            System.out.print(bit + " ");
        }
        System.out.println();

        System.out.print("Encrypted Message: ");

        BigInteger[] cipher = new BigInteger[message.length];

        for (int i = 0; i < message.length; i++) {
            cipher[i] = encrypt(message[i]);
            System.out.print(cipher[i] + " ");
        }

        System.out.println();

        System.out.print("Decrypted Message: ");

        for (BigInteger c : cipher) {
            System.out.print(decrypt(c) + " ");
        }

        System.out.println();
    }
}
```

## How to Execute

### Step 1: Check Java

```bash
java --version
javac --version
```

### Step 2: Compile

```bash
javac GoldwasserMicali.java
```

### Step 3: Run

```bash
java GoldwasserMicali
```

## Sample Output

```text
Goldwasser-Micali Cryptosystem
Public Key  : (209, 17)
Private Key : (11, 19)
Original Message : 1 0 1 1 0
Encrypted Message: ...
Decrypted Message: 1 0 1 1 0
```

The encrypted values may change on every execution because GM is probabilistic.

## Result
The Goldwasser-Micali probabilistic public key cryptosystem was successfully implemented, and the decrypted message matched the original message.

## Important Exam Points
- GM is a probabilistic public-key cryptosystem.
- It encrypts one bit at a time.
- Public key: `(n, y)`.
- Private key: `(p, q)`.
- Randomness makes repeated encryption produce different ciphertexts.
