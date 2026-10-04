# Experiment 4 — Subset Sum

## AIM

To implement the subset sum problem and determine whether a subset of given numbers has a sum equal to a specified target.

## Tools Required

- Java JDK 8 or later
- Command Prompt / Terminal
- Java IDE (optional)

## Concept

The subset sum problem asks whether a subset of the given numbers can produce a specified target sum.

Example:

```text
Set = {2, 3, 7, 8, 10}
Target = 11
```

A valid subset is:

```text
3 + 8 = 11
```

For `n` elements, there can be up to `2^n` possible subsets.

## Procedure

1. Read the number of elements.
2. Read the elements.
3. Read the target sum.
4. Recursively consider including or excluding each element.
5. Check whether the target can be obtained.
6. Display the result.

## Source Code

Save as `SubsetSum.java`.

```java
import java.util.*;

public class SubsetSum {

    static boolean subsetSum(int[] a, int n, int target) {

        if (target == 0)
            return true;

        if (n == 0)
            return false;

        if (a[n - 1] > target)
            return subsetSum(a, n - 1, target);

        return subsetSum(a, n - 1, target) ||
               subsetSum(a, n - 1, target - a[n - 1]);
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of elements: ");
        int n = sc.nextInt();

        int[] a = new int[n];

        System.out.println("Enter elements:");

        for (int i = 0; i < n; i++)
            a[i] = sc.nextInt();

        System.out.print("Enter target sum: ");
        int target = sc.nextInt();

        if (subsetSum(a, n, target))
            System.out.println(
                    "Subset with given sum exists.");
        else
            System.out.println(
                    "Subset with given sum does not exist.");
    }
}
```

## Steps to Execute

### Step 1 — Compile

```bash
javac SubsetSum.java
```

### Step 2 — Run

```bash
java SubsetSum
```

### Step 3 — Enter Example Input

```text
Enter number of elements: 5
Enter elements:
2 3 7 8 10
Enter target sum: 11
```

### Step 4 — Observe Output

```text
Subset with given sum exists.
```

because:

```text
3 + 8 = 11
```

## Viva Points

- Subset sum is a combinatorial problem.
- Each element has two choices: include or exclude.
- The basic recursive solution has exponential time complexity.
- There can be `2^n` subsets.
