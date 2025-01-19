# Java Developer Test

## 1. Write a Java program to generate anagrams from a set of distinct letters

You must create a utility function for a text-processing application that generates all possible anagrams of a given set of distinct letters.

### Java Code
```java
import java.util.*;

public class AnagramGenerator {
    public static List<String> generateAnagrams(String input) {
        List<String> result = new ArrayList<>();
        if (input == null || input.isEmpty() || !input.matches("[a-zA-Z]+")) {
            throw new IllegalArgumentException("Invalid input! Use only letters and avoid empty strings.");
        }
        generateAnagramsHelper("", input, result);
        return result;
    }

    private static void generateAnagramsHelper(String prefix, String remaining, List<String> result) {
        if (remaining.isEmpty()) {
            result.add(prefix);
        } else {
            for (int i = 0; i < remaining.length(); i++) {
                generateAnagramsHelper(
                    prefix + remaining.charAt(i), 
                    remaining.substring(0, i) + remaining.substring(i + 1), 
                    result
                );
            }
        }
    }

    public static void main(String[] args) {
        System.out.println(generateAnagrams("abc"));
    }
}
```

## 2. Example of overriding the `equals()` method

```java
import java.util.Objects;

public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Person person = (Person) obj;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

## 3. Design pattern to decouple code from a third-party library

The **Adapter Pattern** is a common approach to decouple the code from an external library, allowing its future replacement.

```java
public interface PaymentProcessor {
    void processPayment(double amount);
}

public class StripeAdapter implements PaymentProcessor {
    private StripeAPI stripe;

    public StripeAdapter(StripeAPI stripe) {
        this.stripe = stripe;
    }

    @Override
    public void processPayment(double amount) {
        stripe.charge(amount);
    }
}
```

## 4. Techniques to prevent SQL Injection

```java
PreparedStatement stmt = conn.prepareStatement("SELECT * FROM users WHERE username = ?");
stmt.setString(1, username);
```

## 5. Diagnosing and optimizing a batch process

- **Identify bottlenecks using logs and profiling**
- **Optimize SQL queries (indexing, efficient joins)**
- **Improve execution logic using multithreading**
- **Enhance file transfer (compression, parallelization)**

## 6. SQL Queries

### a) Salespersons without orders for Samsonic
```sql
SELECT s.name 
FROM Salesperson s 
WHERE s.id NOT IN (
    SELECT o.salesperson_id FROM Orders o
    JOIN Customer c ON o.customer_id = c.id
    WHERE c.name = 'Samsonic'
);
```

### b) Append `*` to salespersons with 2+ orders
```sql
UPDATE Salesperson 
SET Name = Name || '*' 
WHERE id IN (SELECT salesperson_id FROM Orders GROUP BY salesperson_id HAVING COUNT(*) >= 2);
```

### c) Remove salespersons who sold to Jackson
```sql
DELETE FROM Salesperson 
WHERE id IN (
    SELECT salesperson_id FROM Orders o
    JOIN Customer c ON o.customer_id = c.id
    WHERE c.city = 'Jackson'
);
```

### d) Total sales amount per salesperson
```sql
SELECT s.name, COALESCE(SUM(o.amount), 0) AS total_sales
FROM Salesperson s
LEFT JOIN Orders o ON s.id = o.salesperson_id
GROUP BY s.name;
```

## 7. Use case for plant registration

**Rules:**
- Code must be unique.
- Only admins can delete.

**Tests**
1. Create a plant with a unique code (✔)
2. Try creating a plant with a duplicate code (❌)
3. Non-admin user attempting to delete (❌)

## 8. User registration tests

**Tests:**
1. Create a valid user.
2. Create a user with a duplicate email.
3. Delete a user without permission.

**Unit test:**
```java
@Test
public void testCreateUser_Success() {
    User user = new User("John", "john@example.com");
    assertEquals("John", user.getName());
}
```