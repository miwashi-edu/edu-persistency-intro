# edu-persistency-intro


## No Exception Handling

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class App {
    private static final String URL = "jdbc:mysql://localhost:3306/test";
    private static final String USER = "test";
    private static final String PASSWORD = "test";

    private static Connection connection;

    public static void main(String[] args) throws SQLException {
        connection = DriverManager.getConnection(URL, USER, PASSWORD);
        System.out.println("Connected to the database successfully!");
        connection.close();
    }
}
```

## The Exception Handling


```java
public class App {
    public static void main(String[] args) {
        try {
            // Place the code you want to execute here
            System.out.println("Code executed successfully!");
        } catch (ClassNotFoundException e) {
            System.out.println("Class not found exception occurred.");
            e.printStackTrace();
        } catch (SQLException e) {
            System.out.println("SQL exception occurred.");
            e.printStackTrace();
        } finally {
            // Place cleanup or final actions here
            System.out.println("Execution completed.");
        }
    }
}
```



