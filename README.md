# edu-persistency-intro


## Functions

## main

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.ResultSet;

public class App {
    // JDBC URL, username, and password of MySQL server
    private static final String URL = "jdbc:mysql://localhost:3306/test";
    private static final String USER = "test";
    private static final String PASSWORD = "test";

    private static Connection connection;

    public static void main(String[] args) throws SQLException {
        try {
            connect();
            dropTableIfExists();
            createTable();
            insertIntoTable("Alice");
            insertIntoTable("Bob");
            selectAllFromTable();
        } finally {
            if (connection != null) {
                connection.close();
            }
        }
    }
}
```

## connect

```java
private static void connect() throws SQLException {
    connection = DriverManager.getConnection(URL, USER, PASSWORD);
    System.out.println("Connected to the database successfully!");
}
```

## drop if exists

```java
private static void dropTableIfExists() throws SQLException {
    String sql = "DROP TABLE IF EXISTS User";
    try (Statement stmt = connection.createStatement()) {
        stmt.executeUpdate(sql);
        System.out.println("Table 'User' dropped if it existed.");
    }
}
```

## create table

```java
private static void createTable() throws SQLException {
    String sql = """
        CREATE TABLE User (
            id INT AUTO_INCREMENT PRIMARY KEY,
            name VARCHAR(50) NOT NULL
        )
    """;
    try (Statement stmt = connection.createStatement()) {
        stmt.executeUpdate(sql);
        System.out.println("Table 'User' created successfully.");
    }
}
```

## insert into

```java
private static void insertIntoTable(String name) throws SQLException {
    String sql = String.format("INSERT INTO User (name) VALUES ('%s')", name);
    try (Statement stmt = connection.createStatement()) {
        stmt.executeUpdate(sql);
        System.out.println("Inserted user: " + name);
    }
}
```

## select all

```java
private static void selectAllFromTable() throws SQLException {
    String sql = "SELECT * FROM User";
    try (Statement stmt = connection.createStatement();
         ResultSet rs = stmt.executeQuery(sql)) {

        System.out.println("Users in 'User' table:");
        while (rs.next()) {
            int id = rs.getInt("id");
            String name = rs.getString("name");
            System.out.println("ID: " + id + ", Name: " + name);
        }
    }
}
```
