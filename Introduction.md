# Introduction to maven 

- Maven is primarily used for simplifying the management of project builds, dependencies, and project structure, enabling developers to focus more on writing code and less on managing these aspects manually.


- **archetype**:- In Maven, an archetype is a template that allows users to create new Maven projects


- maven uses **Convention over Configuration** is a software design principle that aims to reduce the number of decisions developers need to make, by providing sensible defaults for common tasks. In other words, it means that if you follow a set of established conventions (i.e., default practices or structures), you don’t have to configure everything manually. The framework or tool will just "work" based on those conventions, and you only need to override the defaults when necessary.

## What does Maven do ? 

### **Dependency Management**
   - Maven automatically manages libraries and dependencies by fetching them from repositories (e.g., Maven Central).
   - **Example:** Declare dependencies (like Log4j) in `pom.xml`, and Maven downloads and manages them.

### 2**Build Lifecycle Management**
   - Maven defines standard build phases: `compile`, `test`, `package`, `install`, `deploy`.
   - **Example:** Running `mvn install` compiles code, runs tests, packages it (JAR/WAR), and installs it locally.

### **Project Structure**
   - Promotes a standard directory structure for projects:
     ```
      my-project/
      ├── pom.xml             (Project Object Model file)
      ├── src/
      │   ├── main/
      │   │   ├── java/       (Java source code)
      │   │   └── resources/  (Non-code resources)
      │   └── test/
      │       └── java/       (Test source code)
      └── target/             (Compiled files and artifacts)

     ```
   - **Benefits:** Easier collaboration and understanding of project layout.

### **Plugins and Goals**
   - Maven uses plugins for tasks like compiling, testing, generating docs, and deployment.
   - **Example:** `maven-compiler-plugin` (compiles code), `maven-surefire-plugin` (runs tests).

### **Centralized Repository Management**
   - Maven fetches dependencies from a central repository (e.g., Maven Central).
   - **Example:** Add `Apache Commons` to `pom.xml`, and Maven downloads the required JAR.

### **Multi-Module Projects**
   - Supports complex projects with multiple modules (sub-projects).
   - **Benefit:** Centralized parent POM to manage dependencies and builds for child modules.

### **Continuous Integration (CI)**
   - Maven integrates easily with CI tools (Jenkins, Travis, CircleCI) to automate builds and deployments.


## coordinates in Maven


In Maven, "coordinates" refer to a unique identifier for a dependency (library or project) that Maven uses to locate and manage that dependency. The coordinates consist of four main parts:

1. **Group ID**: This is the unique identifier for the group or organization that maintains the project (like a namespace). It's usually in reverse domain name format (e.g., `org.apache.maven`).
   
2. **Artifact ID**: This is the name of the specific project or library (artifact) that you're using (e.g., `maven-core`).

3. **Version**: This is the specific version of the library or artifact you want to use (e.g., `3.9.0`).

4. **Packaging** (optional): This specifies the type of artifact (e.g., `jar`, `war`, `pom`). Most of the time, it's a `.jar` file, but other types of packaging can be used depending on the project.

### Example:
```xml
<dependency>
  <groupId>org.apache.maven</groupId>
  <artifactId>maven-core</artifactId>
  <version>3.9.0</version>
  <scope>compile</scope>
</dependency>
```

In this case, the Maven coordinates are:
- **Group ID**: `org.apache.maven`
- **Artifact ID**: `maven-core`
- **Version**: `3.9.0`

Maven uses these coordinates to download the specified library from a repository (like Maven Central) and include it in your project.



## Repositories

In Maven, **repositories** are locations (either online or on your local machine) where Maven stores and retrieves project dependencies (libraries or other artifacts). Think of a repository as a storage space or a "warehouse" for your project's dependencies.

There are two main types of repositories in Maven:

### 1. **Local Repository**
- This is a folder on your own computer where Maven stores downloaded dependencies.
- It’s located in your home directory, usually at `~/.m2/repository` (for Unix-based systems) or `C:\Users\<YourUsername>\.m2\repository` (for Windows).
- The first time Maven needs a dependency, it will download it from a remote repository (like Maven Central) and store it in your local repository. On subsequent builds, Maven will use the locally cached version, which speeds up the build process.

### 2. **Remote Repository**
- These are online servers where Maven hosts libraries and other artifacts. When your project needs a dependency that is not in your local repository, Maven will go to a remote repository to fetch it.
- The most common remote repository is **Maven Central**, which is the default repository that Maven uses.
- Other organizations or companies might have their own private repositories to store custom or proprietary artifacts.

### Example:

- When you build a Maven project, Maven checks your **local repository** first. If the dependency isn't there, it looks in **remote repositories** (like Maven Central).
- If it still can’t find the dependency, Maven will show an error or you can configure it to look in other repositories you specify in the `pom.xml` file.

### Maven's Default Remote Repository:
Maven's default remote repository is **Maven Central**, which contains millions of open-source libraries and artifacts. It's automatically configured in Maven, so you don't need to set it up manually unless you need a custom repository.

### Example of defining a repository in `pom.xml`:
```xml
<repositories>
  <repository>
    <id>my-repo</id>
    <url>https://mycompany.repo.com/maven2</url>
  </repository>
</repositories>
```

In this example, Maven will look in `https://mycompany.repo.com/maven2` for any dependencies not found in the local or default repositories.

### Summary:
- **Local repository**: Where Maven stores dependencies on your machine.
- **Remote repository**: Where Maven downloads dependencies from (e.g., Maven Central).
- **Repositories** help Maven manage and organize the libraries your project needs.



**when you specifiy dependencies in pom.xml maven will also download the related dependencies as well how?**

maven for particular dependecies maven has pom files which has their dependencies as well 

means go to ~/.m2/repository/org/junit/  -> inside dependecies you found that dependencies pom.xml which has their dependencies based on that it download all dependencies 


## Scopes in maven

In Maven, **scope** is an important concept related to the visibility and lifecycle of dependencies within a project. It defines where and when a dependency is available in different parts of the Maven build lifecycle. The scope of a dependency impacts how and when it gets included in the final build and its availability during compilation, testing, and runtime.

Here’s an explanation of each scope, along with examples and use cases:

### 1. **compile**
- **Scope**: This is the default scope if no scope is specified. Dependencies with the `compile` scope are available in all phases of the build lifecycle (compilation, testing, and runtime).
- **Use Case**: You use this scope for dependencies that are needed for both compiling and running the application.
  
  **Example**:
  ```xml
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.3.0</version>
    <scope>compile</scope>
  </dependency>
  ```
  
  **Use Case Example**: A library that is essential for the project to function, like Spring or Hibernate, would be included with the `compile` scope.

### 2. **provided**
- **Scope**: This scope indicates that the dependency is required during the compilation and testing phases but is provided by the runtime environment (i.e., it will not be included in the final packaged artifact like a WAR or JAR).
- **Use Case**: Used for dependencies that are already provided by the container or runtime environment (e.g., servlet API in a web application).
  
  **Example**:
  ```xml
  <dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
    <scope>provided</scope>
  </dependency>
  ```
  
  **Use Case Example**: In a Java web application, the servlet API is often provided by the servlet container (e.g., Tomcat), so it’s marked as `provided` to avoid bundling it in the final artifact.

### 3. **runtime**
- **Scope**: Dependencies with this scope are required at runtime but not during compilation. They are available only when the project is running, not when it is being built or tested.
- **Use Case**: Useful for libraries or frameworks that are required only when the application runs, such as JDBC drivers or logging libraries.
  
  **Example**:
  ```xml
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.23</version>
    <scope>runtime</scope>
  </dependency>
  ```
  
  **Use Case Example**: A MySQL JDBC driver would be included with the `runtime` scope, since it’s only needed at runtime when the application connects to the database.

### 4. **test**
- **Scope**: Dependencies with the `test` scope are only available during the test phase. These dependencies are not included in the final artifact and are excluded from compilation and runtime unless explicitly included in test code.
- **Use Case**: Used for libraries and frameworks that are only needed for unit tests or integration tests (e.g., JUnit, TestNG).
  
  **Example**:
  ```xml
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
  </dependency>
  ```
  
  **Use Case Example**: You would include JUnit or other testing frameworks as `test` scope dependencies because they are required only during the testing phase.

### 5. **system**
- **Scope**: Dependencies with the `system` scope are similar to `provided`, but instead of being retrieved from a repository, they must be explicitly defined using a path on the local file system. This scope is typically used for dependencies that are not available in public repositories and are provided locally.
- **Use Case**: Rarely used but can be useful when working with proprietary or custom libraries that need to be explicitly provided from a local file system.
  
  **Example**:
  ```xml
  <dependency>
    <groupId>com.example</groupId>
    <artifactId>custom-library</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/lib/custom-library-1.0.jar</systemPath>
  </dependency>
  ```
  
  **Use Case Example**: If you have a custom JAR file stored locally and need to include it in your Maven build, you would use the `system` scope and define the path to the JAR file.

### 6. **import**
- **Scope**: This scope is used in Maven’s BOM (Bill of Materials) feature, where you want to import a set of dependencies from another POM file. It is only valid in the `<dependencyManagement>` section.
- **Use Case**: Useful when you are managing versions for multiple dependencies in a central place.
  
  **Example**:
  ```xml
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.3.4.RELEASE</version>
        <scope>import</scope>
        <type>pom</type>
      </dependency>
    </dependencies>
  </dependencyManagement>
  ```
  
  **Use Case Example**: You would use the `import` scope to import a set of dependency versions from a parent POM, like in Spring Boot, to manage versions for all dependencies centrally.

### Summary of Maven Scopes:
- **compile**: Available everywhere (default).
- **provided**: Available at compile and test time, but not in the final artifact (provided by the runtime).
- **runtime**: Available only at runtime.
- **test**: Available only during testing.
- **system**: Similar to `provided`, but dependencies are specified via a system path.
- **import**: Used to import dependency versions from another POM (used in dependency management).

Each scope helps you manage dependencies in different phases of your build lifecycle and can be crucial for optimizing your project’s build process and artifact size.