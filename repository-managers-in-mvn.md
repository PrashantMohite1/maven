

## **Repository Manager in Maven**

A **repository manager** (like **Nexus** or **Artifactory**) is a tool used to manage, store, and organize dependencies (JAR files) in Maven projects. It acts as a central hub for storing and retrieving both public and private artifacts used across teams and projects.

#### **Key Functions of a Repository Manager:**
1. **Centralized Artifact Storage**: Stores both public (e.g., from Maven Central) and private artifacts in one place, making them easily accessible for multiple projects and teams.
2. **Local Caching**: Caches remote dependencies locally, improving build performance by reducing network calls.
3. **Version Control**: Manages different versions of dependencies (both snapshots and releases) to ensure consistency across builds.
4. **Artifact Deployment**: Allows teams to deploy their own artifacts (e.g., private libraries) to a shared repository.
5. **Access Control**: Controls who can upload or download specific artifacts, improving security and governance.

#### **Types of Repositories:**
1. **Local Repository**: Stores artifacts on a developer’s machine (typically in `~/.m2` folder).
2. **Remote Repository**: Stores public artifacts available over the internet (e.g., Maven Central).
3. **Internal Repository**: A private repository used by teams to store proprietary or custom artifacts (can be hosted by Nexus or Artifactory).we can also deployed this our internal repo
  on cloud and then use for our organisation 

---

### **Snapshot and Release Management**

Maven distinguishes between **snapshots** (development versions) and **releases** (stable versions). Repository managers handle these versions differently:

1. **Snapshot Versions**:
   - Mutable, development versions that can change frequently (e.g., `1.0-SNAPSHOT`).
   - Repository managers store and track the latest snapshot version, allowing teams to fetch the most recent build.
   - When a new snapshot is deployed, the previous one can be overwritten with a newer version.

2. **Release Versions**:
   - Immutable, stable versions that are meant for production (e.g., `1.0.0`).
   - Once a release is deployed, it is **never** changed. If you try to upload the same version again, the repository manager will reject it.
   - This ensures that everyone using the release version gets the exact same artifact.

#### **Repository Manager’s Role with Snapshots and Releases**:
- **Separation**: Repository managers separate **snapshot** and **release** versions into different repositories (e.g., `maven-releases/` for stable versions and `maven-snapshots/` for development versions).
- **Caching**: Snapshots can be updated frequently, so the repository manager always serves the latest version. Releases, however, remain fixed and are served as-is.
- **Version Management**: Helps maintain consistency by preventing changes to release versions and ensuring that developers always get the correct snapshot or release version as needed.


### **Why Use a Repository Manager?**
1. **Centralized Dependency Management**: Easy access to shared dependencies for multiple projects and teams.
2. **Improved Build Speed**: Caches artifacts locally and reduces dependency downloads, speeding up builds.
3. **Better Version Control**: Keeps track of both mutable snapshots and immutable releases, ensuring stability and predictability in builds.
4. **Security and Governance**: Manages access and permissions to control who can upload/download artifacts.


## To use your self-hosted Nexus repository in your Maven project:


1. **In `pom.xml`**:  
   Add Nexus repositories under `<repositories>` for dependencies and `<distributionManagement>` for deploying artifacts.

2. **In `settings.xml`** (`~/.m2/settings.xml` for user-specific or `{Maven_Home}/conf/settings.xml` for global):  
   **Note**: The `settings.xml` is **not created by default**. You need to create it manually.  
   - Add your Nexus authentication credentials under `<servers>`.
   - Add a **mirror** to route all Maven requests to your Nexus repository.

3. **Run Maven deploy**:  
   Use `mvn clean deploy` to upload artifacts to Nexus.




---
---
### **Detailed Guide: Using Your Self-Hosted Nexus Repository in a Maven Project**

---

#### **Step 1: Add Nexus Repository to `pom.xml`**

You need to tell Maven where to look for dependencies and where to deploy artifacts. This is done by configuring the `<repositories>` and `<distributionManagement>` sections in your `pom.xml`.

1. **Add Nexus repository for dependencies** under `<repositories>`:
   - This configuration tells Maven to fetch dependencies from your Nexus repository.

   ```xml
   <repositories>
       <repository>
           <id>nexus-repo</id>
           <url>http://localhost:8081/repository/maven-public/</url> <!-- Change URL to your Nexus repo -->
       </repository>
   </repositories>
   ```

   - **`id`**: This is the identifier for the repository. It can be any name you choose (e.g., `nexus-repo`).
   - **`url`**: The URL of your Nexus repository. If you're using a public proxy repository (e.g., Maven Central), it could be something like `http://localhost:8081/repository/maven-central/`. For your custom repositories, use the appropriate URL like `http://localhost:8081/repository/maven-releases/` or `http://localhost:8081/repository/maven-snapshots/`.

2. **Add Nexus repository for artifact deployment** under `<distributionManagement>`:
   - This tells Maven where to deploy your build artifacts (JARs, WARs, etc.).

   ```xml
   <distributionManagement>
       <repository>
           <id>nexus-releases</id>
           <url>http://localhost:8081/repository/maven-releases/</url> <!-- Your release repository URL -->
       </repository>
       <snapshotRepository>
           <id>nexus-snapshots</id>
           <url>http://localhost:8081/repository/maven-snapshots/</url> <!-- Your snapshot repository URL -->
       </snapshotRepository>
   </distributionManagement>
   ```

   - **`repository`**: For stable release versions (e.g., `1.0.0`).
   - **`snapshotRepository`**: For development snapshot versions (e.g., `1.0.0-SNAPSHOT`).

---

#### **Step 2: Configure `settings.xml` for Authentication and Mirror**

Maven needs to authenticate with your Nexus repository when deploying artifacts. You also need to configure a **mirror** to route all repository requests through Nexus (for both fetching and deploying).

1. **Create `settings.xml`** (if it doesn't exist):
   - By default, Maven does **not create** the `settings.xml` file in the `~/.m2` directory. If it's missing, you need to manually create it.

   ```bash
   touch ~/.m2/settings.xml
   ```

2. **Add Authentication Details** for Nexus:
   - You'll configure your **Nexus credentials** (username and password or token) in the `<servers>` section of `settings.xml`. These credentials are used when Maven interacts with your Nexus server.

   ```xml
   <settings>
       <servers>
           <server>
               <id>nexus-releases</id>
               <username>your-username</username> <!-- Replace with your Nexus username -->
               <password>your-password</password> <!-- Replace with your Nexus password -->
           </server>
           <server>
               <id>nexus-snapshots</id>
               <username>your-username</username> <!-- Replace with your Nexus username -->
               <password>your-password</password> <!-- Replace with your Nexus password -->
           </server>
       </servers>
   </settings>
   ```

   - **`id`**: This should match the `<id>` you use in the `pom.xml` file under the `<distributionManagement>` section.
   - **`username` and `password`**: These are your Nexus credentials. You can also use an **authentication token** instead of a password for security.

3. **Add Mirror for Nexus Repository** (optional but recommended):
   - To route all Maven requests (for dependencies and artifact deployments) through your Nexus repository, you need to configure a **mirror** in the `settings.xml` file.

   ```xml
   <mirrors>
       <mirror>
           <id>nexus-mirror</id>
           <mirrorOf>external:http://localhost:8081/repository/maven-public/</mirrorOf>
           <url>http://localhost:8081/repository/maven-public/</url> <!-- Your Nexus repository URL -->
           <blocked>false</blocked>
       </mirror>
   </mirrors>
   ```

   - **`<mirrorOf>`**: This specifies which repositories this mirror should cover. `external` means it will apply to any external repository (e.g., Maven Central).
   - **`<url>`**: This should be the URL of your Nexus repository (e.g., `http://localhost:8081/repository/maven-public/`).
   - **`<blocked>`**: Set to `false` to allow Maven to use this mirror.

---

#### **Step 3: Deploy Artifacts to Nexus**

Once your `pom.xml` and `settings.xml` files are configured, you can deploy your artifacts to Nexus.

1. **Run Maven Deploy Command**:
   - To deploy your artifacts (e.g., JARs, WARs) to Nexus, use the following Maven command:

   ```bash
   mvn clean deploy
   ```

   This command will:
   - **Clean**: Clean the project (delete target directory).
   - **Deploy**: Build the project and deploy the artifact (JAR, WAR, etc.) to the configured Nexus repository (`nexus-releases` or `nexus-snapshots`).

2. **Deploy Snapshot Artifacts**:
   - If you're deploying **snapshots** (e.g., `1.0.0-SNAPSHOT`), Maven will upload them to the `nexus-snapshots` repository.
   - Snapshots are versioned builds that can be overwritten, so each deployment will store the latest build of the snapshot.

3. **Deploy Release Artifacts**:
   - If you're deploying **release versions** (e.g., `1.0.0`), Maven will upload them to the `nexus-releases` repository.
   - Releases are immutable, meaning once a release version is uploaded, it cannot be overwritten.

---

#### **Step 4: Verify Artifact Deployment**

1. **Check in Nexus UI**:
   - After deploying, go to the Nexus UI (`http://localhost:8081` or your server's URL).
   - Under the **"Browse"** section, navigate to the repository (either `maven-releases` or `maven-snapshots`) and verify that your artifact is listed.

2. **Dependency Resolution**:
   - If you have configured the `<repositories>` section correctly in `pom.xml`, Maven will fetch dependencies from your Nexus repository (if they are hosted there) or proxy them from public repositories like Maven Central through Nexus.

---

### **Final Summary:**

1. **In `pom.xml`**:  
   Add Nexus repositories for resolving dependencies and deploying artifacts.

2. **In `settings.xml`** (`~/.m2/settings.xml` for user-specific or `{Maven_Home}/conf/settings.xml` for global):  
   - **Create `settings.xml` if it doesn't exist**.  
   - Add Nexus credentials under `<servers>`.  
   - Optionally, add a mirror to route all requests through Nexus.

3. **Run `mvn clean deploy`** to deploy artifacts to Nexus.

---

This detailed guide should give you everything you need to set up and use a self-hosted Nexus repository with Maven! Let me know if you have any further questions.
