

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

---

### **Why Use a Repository Manager?**
1. **Centralized Dependency Management**: Easy access to shared dependencies for multiple projects and teams.
2. **Improved Build Speed**: Caches artifacts locally and reduces dependency downloads, speeding up builds.
3. **Better Version Control**: Keeps track of both mutable snapshots and immutable releases, ensuring stability and predictability in builds.
4. **Security and Governance**: Manages access and permissions to control who can upload/download artifacts.


## To use your self-hosted Nexus repository in your Maven project:

1. **In `pom.xml`**:  
   Add the Nexus repository under `<repositories>` for dependencies and under `<distributionManagement>` for deploying artifacts.

2. **In `settings.xml`** (`~/.m2/settings.xml` for user-specific or `{Maven_Home}/conf/settings.xml` for global): Add your Nexus authentication credentials under `<servers>` in the `settings.xml`.

   **Note**: The `settings.xml` is **not created by default**. You will need to create it manually if it doesn't exist.  

3. **Run Maven deploy**:  
   Use `mvn clean deploy` to upload artifacts to Nexus.

