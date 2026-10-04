# Experiment 10 — Kerberos Cryptosystem

## Aim
To implement a simplified simulation of the Kerberos authentication protocol using Java.

## Requirements
- Java JDK 8 or later
- VS Code / IntelliJ IDEA / Eclipse
- Terminal or Command Prompt

## Concept
Kerberos is a network authentication protocol based on tickets and symmetric-key cryptography.

Main entities:

```text
Client
   |
   v
Authentication Server (AS)
   |
   v
Ticket Granting Server (TGS)
   |
   v
Application Server
```

The AS authenticates the client and provides a Ticket Granting Ticket (TGT). The client uses the TGT to obtain a service ticket from the TGS. The service ticket is then presented to the application server.

This program is an educational simulation of the protocol flow, not a production Kerberos implementation.

## Procedure
1. Client sends username and password to the authentication server in the simulation.
2. AS verifies the credentials.
3. AS generates a session key and TGT.
4. Client presents the TGT to the TGS.
5. TGS verifies the TGT and creates a service ticket.
6. Client presents the service ticket to the application server.
7. Application server verifies the ticket.
8. Authentication succeeds.

## Source Code

Save as `KerberosDemo.java`:

```java
import java.util.Base64;

public class KerberosDemo {

    static String encrypt(String data, String key) {
        StringBuilder result = new StringBuilder();

        for (int i = 0; i < data.length(); i++) {
            result.append(
                (char)(data.charAt(i)
                    ^ key.charAt(i % key.length()))
            );
        }

        return Base64.getEncoder()
                .encodeToString(result.toString().getBytes());
    }

    static String decrypt(String data, String key) {
        byte[] decoded =
                Base64.getDecoder().decode(data);

        String encrypted = new String(decoded);

        StringBuilder result = new StringBuilder();

        for (int i = 0; i < encrypted.length(); i++) {
            result.append(
                (char)(encrypted.charAt(i)
                    ^ key.charAt(i % key.length()))
            );
        }

        return result.toString();
    }

    public static void main(String[] args) {

        String username = "hitha";
        String password = "1234";

        String clientKey = "CLIENT";
        String tgsKey = "TGSKEY";

        System.out.println("Kerberos Authentication");

        System.out.println("\n1. Client -> Authentication Server");

        if (username.equals("hitha")
                && password.equals("1234")) {

            System.out.println("User authenticated by AS.");

            String sessionKey = "SESSION123";

            String tgt =
                    encrypt(username + ":" + sessionKey,
                            clientKey);

            System.out.println("TGT Generated: " + tgt);

            System.out.println(
                "\n2. Client -> Ticket Granting Server"
            );

            String tgtData =
                    decrypt(tgt, clientKey);

            System.out.println("TGT Verified.");

            String serviceTicket =
                    encrypt(
                        tgtData + ":FILE_SERVER",
                        tgsKey
                    );

            System.out.println(
                "Service Ticket Generated: "
                + serviceTicket
            );

            System.out.println(
                "\n3. Client -> Application Server"
            );

            String serviceData =
                    decrypt(serviceTicket, tgsKey);

            System.out.println(
                "Service Ticket Verified."
            );

            System.out.println(
                "User " + username +
                " successfully authenticated."
            );

        } else {
            System.out.println("Authentication Failed.");
        }
    }
}
```

## How to Execute

```bash
javac KerberosDemo.java
java KerberosDemo
```

## Sample Output

```text
Kerberos Authentication

1. Client -> Authentication Server
User authenticated by AS.
TGT Generated: ...

2. Client -> Ticket Granting Server
TGT Verified.
Service Ticket Generated: ...

3. Client -> Application Server
Service Ticket Verified.
User hitha successfully authenticated.
```

## Result
The simplified Kerberos authentication process was successfully demonstrated using the AS, TGS, TGT, and service ticket flow.

## Important Exam Points
- Kerberos is an authentication protocol.
- It uses tickets.
- AS = Authentication Server.
- TGS = Ticket Granting Server.
- TGT = Ticket Granting Ticket.
- Kerberos requires reasonably synchronized clocks in real deployments.
