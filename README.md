# edu-persistency-intro


## Create Myswl Container

```bash
mkdir ~/mysql_data 
docker run --name mysql-container \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=test \
  -e MYSQL_USER=test \
  -e MYSQL_PASSWORD=test \
  -v ~/mysql_data:/var/lib/mysql \
  -p 3306:3306 \
  -d mysql:8.0
```

## Java Connect

```bash
cd ~
cd ws
mkdir jdbc-test
cd jdbc-test
git init
curl -o .gitignore https://raw.githubusercontent.com/github/gitignore/main/Java.gitignore
curl -o mysql-connector-java.jar https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.28/mysql-connector-java-8.0.28.jar
git add .
touch App.java
git commit -m "Initial Commit"
```

```bash
cat > App.java << 'EOF'
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class App {
    // JDBC URL, username and password of MySQL server
    private static final String URL = "jdbc:mysql://localhost:3306/test";
    private static final String USER = "test";
    private static final String PASSWORD = "test";

    // JDBC variables for opening and managing connection
    private static Connection connection;

    public static void main(String[] args) {
        try {
            // Load MySQL JDBC driver
            Class.forName("com.mysql.cj.jdbc.Driver");
            
            // Establish connection using the DriverManager
            connection = DriverManager.getConnection(URL, USER, PASSWORD);
            if (connection != null) {
                System.out.println("Connected to the database successfully!");
            } else {
                System.out.println("Failed to make connection!");
            }
        } catch (ClassNotFoundException e) {
            System.out.println("MySQL JDBC Driver not found.");
            e.printStackTrace();
        } catch (SQLException e) {
            System.out.println("Connection Failed! Check output console.");
            e.printStackTrace();
        } finally {
            // Close connection
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
EOF
```

## Compile and Run App

```bash
javac -cp .:mysql-connector-java.jar App.java
java -cp .:mysql-connector-java.jar App
```


## Reset and Restart to last commit

```bash
git reset --hard
git clean -df
```

