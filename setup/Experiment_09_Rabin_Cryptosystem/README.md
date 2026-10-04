# Experiment 9 — Rabin Cryptosystem

## Aim
To implement the Rabin public-key cryptosystem using Java.

## Requirements
- Java JDK 8 or later
- VS Code / IntelliJ IDEA / Eclipse
- Terminal or Command Prompt

## Concept
The Rabin cryptosystem is a public-key encryption algorithm based on the difficulty of integer factorization.

Choose primes `p` and `q` such that:

`p ≡ 3 (mod 4)` and `q ≡ 3 (mod 4)`

Then:

`n = p × q`

Encryption is:

`c = m² mod n`

During decryption, square roots of `c` modulo `p` and `q` are found and combined using the Chinese Remainder Theorem (CRT).

A ciphertext generally has four possible square roots modulo `n`, so redundancy or padding is normally needed in a practical system.

## Procedure
1. Select primes `p` and `q`.
2. Verify both are 3 modulo 4.
3. Calculate `n = p × q`.
4. Select a plaintext `m`.
5. Calculate `c = m² mod n`.
6. Calculate square roots modulo `p` and `q`.
7. Combine the roots using CRT.
8. Display the possible plaintext values.
9. Verify that the original plaintext is among them.

## Source Code

Save as `RabinCryptosystem.java`:

```java
import java.math.BigInteger;

public class RabinCryptosystem {

    static BigInteger p = BigInteger.valueOf(7);
    static BigInteger q = BigInteger.valueOf(11);
    static BigInteger n = p.multiply(q);

    public static void main(String[] args) {

        BigInteger m = BigInteger.valueOf(20);

        System.out.println("Rabin Cryptosystem");
        System.out.println("p = " + p);
        System.out.println("q = " + q);
        System.out.println("n = " + n);

        BigInteger c = m.multiply(m).mod(n);

        System.out.println("Original Message = " + m);
        System.out.println("Encrypted Message = " + c);

        BigInteger mp =
                c.modPow(
                    p.add(BigInteger.ONE)
                     .divide(BigInteger.valueOf(4)),
                    p
                );

        BigInteger mq =
                c.modPow(
                    q.add(BigInteger.ONE)
                     .divide(BigInteger.valueOf(4)),
                    q
                );

        BigInteger[] rootsP = {
            mp,
            p.subtract(mp)
        };

        BigInteger[] rootsQ = {
            mq,
            q.subtract(mq)
        };

        System.out.println("Possible decrypted messages:");

        BigInteger yp =
                q.multiply(q.modInverse(p));

        BigInteger yq =
                p.multiply(p.modInverse(q));

        for (BigInteger rp : rootsP) {
            for (BigInteger rq : rootsQ) {

                BigInteger result =
                        rp.multiply(yp)
                          .add(rq.multiply(yq))
                          .mod(n);

                System.out.println(result);
            }
        }
    }
}
```

## How to Execute

```bash
javac RabinCryptosystem.java
java RabinCryptosystem
```

## Sample Output

```text
Rabin Cryptosystem
p = 7
q = 11
n = 77
Original Message = 20
Encrypted Message = 15
Possible decrypted messages:
64
20
57
13
```

The order of possible roots may vary.

## Result
The Rabin cryptosystem was successfully implemented. The original plaintext was recovered as one of the possible decrypted roots.

## Important Exam Points
- Public key: `n`.
- Private key: `p, q`.
- Encryption: `c = m² mod n`.
- Security is related to integer factorization.
- Basic Rabin decryption gives four possible roots.
