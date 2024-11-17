### **Lesson 2: Creating and Building a Simple Maven Project**

In this lesson, we'll go through the steps to create a simple Maven project and build it.

#### Step 1: Installing Maven

Before you start using Maven, you need to install it. If you don't have Maven installed yet, follow these steps:

1. **Download Maven**: Go to the [official Apache Maven website](https://maven.apache.org/download.cgi) and download the latest version.
2. **Install Maven**:
   - Extract the downloaded archive to a folder on your system.
   - Set the `MAVEN_HOME` environment variable to the Maven installation directory.
   - Add `MAVEN_HOME/bin` to your system’s `PATH` variable to run Maven commands from anywhere.

To verify that Maven is installed, open a terminal (or command prompt) and run:

```bash
mvn -v
```

You should see the version of Maven installed, along with Java details.

---

#### Step 2: Creating a Maven Project from Command Line

Now, let's create a simple Maven project using Maven's command-line tool.

1. **Open a terminal (or command prompt)**.
2. **Navigate to the directory** where you want to create your project.
3. **Run the following Maven command to create a new project**:

```bash
mvn archetype:generate -DgroupId=com.example -DartifactId=my-project -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

Explanation:
- `archetype:generate`: This is a Maven goal that helps generate a new project based on a template (called an archetype).
- `-DgroupId=com.example`: Defines the group ID (namespace).
- `-DartifactId=my-project`: Defines the project name (artifact).
- `-DarchetypeArtifactId=maven-archetype-quickstart`: Specifies the archetype, which is a basic template for a Java application with a simple `main` method.
- `-DinteractiveMode=false`: Disables interactive prompts, so Maven doesn't ask for inputs during project generation.

After running the command, Maven will create the project with the following structure:

```
my-project
│
├── pom.xml  
└── src
    ├── main
    │   └── java
    │       └── com
    │           └── example
    │               └── App.java
    └── test
        └── java
            └── com
                └── example
                    └── AppTest.java
```

---

#### Step 3: Understanding the Project Structure

Let’s take a quick look at the generated project structure:

- **pom.xml**: The project descriptor file (as we saw earlier).
- **src/main/java/com/example/App.java**: The main Java file with a simple `main` method.
- **src/test/java/com/example/AppTest.java**: A basic JUnit test file to test the `App` class.

---

#### Step 4: Building the Project

To build (compile) your project, run this command from the root of the project (where `pom.xml` is located):

```bash
mvn clean package
```

Explanation:
- `clean`: Cleans the target directory by deleting the compiled files from previous builds.
- `package`: Compiles your code and creates the final packaged output (like a JAR file).

After running the command, Maven will create a `.jar` file inside the `target` directory, usually named `my-project-1.0-SNAPSHOT.jar`.

---

#### Step 5: Running the Application

You can run the application by using the `java -jar` command:

```bash
java -jar target/my-project-1.0-SNAPSHOT.jar
```

This will execute the `App` class, which should print something like:

```
Hello World!
```

---

### What did we cover?
- Installed Maven and set it up.
- Created a Maven project using an archetype.
- Built and packaged the project into a JAR file.

Once you're done with this, let me know, and I’ll guide you through the next lesson! Just ask for the "Next lesson" when you're ready!