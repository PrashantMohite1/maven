# Introduction to maven 

Maven is primarily used for simplifying the management of project builds, dependencies, and project structure, enabling developers to focus more on writing code and less on managing these aspects manually.


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


