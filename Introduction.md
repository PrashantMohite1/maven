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


