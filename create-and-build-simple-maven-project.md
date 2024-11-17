### **Lesson: Creating and Running a Simple Maven Project**

---

### 1. **Installing Maven**

Before starting with Maven, you need to install it.

1. **Download Maven** from the [official website](https://maven.apache.org/download.cgi).
2. **Install Maven**:
   - Extract the downloaded archive.
   - Set the `MAVEN_HOME` environment variable.
   - Add `MAVEN_HOME/bin` to your system's `PATH` variable.
   
To verify installation, run:
```bash
mvn -v
```

---

### 2. **Creating a Simple Maven Project**

**Step 1**: Create a new Maven project using the **quickstart archetype**.

```bash
mvn archetype:generate -DgroupId=com.example -DartifactId=hello-world -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

- This creates a basic Java project with a `main` method.
- `groupId`: Your project’s group (e.g., `com.example`).
- `artifactId`: The project name (e.g., `hello-world`).
- `archetypeArtifactId`: Template for the project (here, `maven-archetype-quickstart`).
- `-DinteractiveMode=false`: Skips interactive prompts during creation.

---

### 3. **Project Structure**

After running the above command, Maven generates the following structure:

```
hello-world
│
├── pom.xml         # Project configuration file
└── src
    └── main
        └── java
            └── com
                └── example
                    └── App.java    # Main Java file
    └── test
        └── java
            └── com
                └── example
                    └── AppTest.java  # Test file
```

---

### 4. **Modifying the `App.java` File**

Open `src/main/java/com/example/App.java` and modify the `main` method to print `"Hello World!"`:

```java
package com.example;

public class App {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

---

### 5. **Configuring `Main-Class` in `pom.xml`**

To run the JAR file with `java -jar`, you need to specify the **main class** in the `pom.xml` file.

1. Open `pom.xml`.
2. Add the following to the `<build>` section to set the `Main-Class`:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.2.0</version>
            <configuration>
                <archive>
                    <manifestEntries>
                        <Main-Class>com.example.App</Main-Class>
                    </manifestEntries>
                </archive>
            </configuration>
        </plugin>
    </plugins>
</build>
```

This ensures that when you run the JAR, Maven knows which class to execute.

---

### 6. **Building the Project**

To build and package the project into a JAR file, run:

```bash
mvn clean package
```

- `clean`: Deletes previously compiled files.
- `package`: Compiles the project and creates the `.jar` file.

After running, the JAR will be in the `target/` folder, named something like `hello-world-1.0-SNAPSHOT.jar`.

---

### 7. **Running the JAR**

Now, you can run your JAR with the `java -jar` command:

```bash
java -jar target/hello-world-1.0-SNAPSHOT.jar
```

This will execute the `com.example.App` class and print:

```
Hello World!
```

---

### **Alternative: Running Without `Main-Class` in Manifest**

If you don't want to specify `Main-Class` in the `pom.xml`, you can run the program using the `-cp` option:

```bash
java -cp target/hello-world-1.0-SNAPSHOT.jar com.example.App
```

This also runs the `com.example.App` class directly.

---

### **Summary**

- **Install Maven** and set up your environment.
- **Create a project** using `mvn archetype:generate`.
- **Modify the `App.java`** to print `"Hello World!"`.
- **Add the `Main-Class`** configuration in `pom.xml` for `java -jar` support.
- **Build** the project: `mvn clean package`.
- **Run** the JAR file: `java -jar target/hello-world-1.0-SNAPSHOT.jar`.

---

