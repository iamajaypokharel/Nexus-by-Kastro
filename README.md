# Nexus-by-Kastro

A beginner-friendly DevOps project demonstrating how to build a Java/Maven project with **Jenkins** and publish the generated Maven artifact to **Nexus Repository**.

## 🚀 Project Overview

This project demonstrates the following CI/CD workflow:

```text
GitHub
   │
   ▼
Jenkins
   │
   ├── Git Checkout
   │
   ├── Maven Compile
   │
   ├── Maven Test
   │
   ├── Maven Package
   │
   └── Maven Deploy
   │
   ▼
Nexus Repository
   │
   └── Maven Release Repository
```

The main purpose of this project is to understand how a Java application can be built using Maven and how the generated `.jar` artifact can be published to a Nexus Repository through Jenkins.

---

## 🛠️ Technologies Used

* **Java**
* **Maven**
* **Jenkins**
* **Nexus Repository**
* **Git**
* **GitHub**
* **Linux / Ubuntu**
* **Groovy** — Jenkins Pipeline

---

## 📁 Project Structure

```text
Nexus-by-Kastro/
│
├── .gitignore
├── Nexus.txt
├── README.md
├── pom.xml
│
└── src/
    └── main/
        └── java/
            └── com/
                └── javaproject/
                    └── Application.java
```

---

## ☕ Java Application

The project contains a simple Java application:

```java
package com.javaproject;

public class Application {

    public static String message() {
        return "Hello from Nexus Maven Project!";
    }

    public static void main(String[] args) {
        System.out.println(message());
    }
}
```

This simple application is used to generate a Maven `.jar` artifact.

---

## 📦 Maven Configuration

The project uses Maven for:

* Compiling Java source code
* Running tests
* Packaging the application
* Deploying the artifact to Nexus

The main Maven configuration is located in:

```text
pom.xml
```

The project uses:

```text
Group ID:       com.javaproject
Artifact ID:    database_service_project
Version:        0.0.5
Packaging:      jar
```

---

## 🔨 Maven Commands

### Compile

```bash
mvn compile
```

Compiles the Java source code.

### Test

```bash
mvn test
```

Runs the project's tests.

> Currently, this project does not contain test classes, so Maven may report `No tests to run` while the build still succeeds.

### Package

```bash
mvn package
```

Creates the `.jar` file inside the `target/` directory.

Example:

```text
target/database_service_project-0.0.5.jar
```

### Deploy

```bash
mvn deploy
```

Uploads the generated Maven artifact to the configured Nexus Repository.

---

# 🔄 Jenkins CI/CD Pipeline

Jenkins is used to automate the Maven build and deployment process.

The Jenkins pipeline performs the following stages:

```text
1. Git Checkout
       ↓
2. Compilation
       ↓
3. Testing
       ↓
4. Package
       ↓
5. Deploy Artifacts
       ↓
6. Nexus Repository
```

## Jenkinsfile

```groovy
pipeline {
    agent any

    tools {
        maven 'maven3'
    }

    stages {

        stage('Git checkout ajay') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/iamajaypokharel/Nexus-by-Kastro'
            }
        }

        stage('Compilation') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Testing') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Deploy artifacts ajayman') {
            steps {
                withMaven(
                    globalMavenSettingsConfig: 'settings.xml',
                    jdk: 'jdk17',
                    maven: 'maven3',
                    traceability: true
                ) {
                    sh 'mvn deploy'
                }
            }
        }
    }
}
```

---

# 📤 Nexus Repository

Nexus Repository is used as the artifact repository.

The Maven project is configured to deploy artifacts to:

```text
maven-releases
```

and snapshots to:

```text
maven-snapshots
```

The repository configuration is defined inside `pom.xml` using:

```xml
<distributionManagement>
    <repository>
        <id>maven-releases</id>
        <url>http://YOUR-NEXUS-IP:8081/repository/maven-releases/</url>
    </repository>

    <snapshotRepository>
        <id>maven-snapshots</id>
        <url>http://YOUR-NEXUS-IP:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

> Replace `YOUR-NEXUS-IP` with the address of your Nexus server. Avoid committing Nexus usernames or passwords to GitHub.

---

# 🔐 Maven Settings

Jenkins uses a Maven `settings.xml` file to authenticate with Nexus.

The credentials should be stored securely in Jenkins rather than directly inside `pom.xml`.

The repository IDs in `settings.xml` should match the IDs in `pom.xml`:

```text
maven-releases
maven-snapshots
```

---

# 🧪 Jenkins Build

After configuring the Jenkins pipeline, Jenkins performs:

```bash
mvn compile
```

```bash
mvn test
```

```bash
mvn package
```

```bash
mvn deploy
```

If all stages are successful, the generated artifact is uploaded to Nexus.

---

# 📦 Artifact

The generated artifact is:

```text
database_service_project-0.0.5.jar
```

Maven creates it in:

```text
target/
```

After deployment, Nexus stores the artifact under the Maven release repository.

The repository structure follows the Maven coordinate format:

```text
com/
└── javaproject/
    └── database_service_project/
        └── 0.0.5/
            ├── database_service_project-0.0.5.jar
            └── database_service_project-0.0.5.pom
```

---

# 🎯 Learning Objectives

This project helps demonstrate:

* Git and GitHub
* Maven project structure
* Maven lifecycle
* Maven compilation
* Maven testing
* Maven packaging
* Maven artifact management
* Jenkins Pipeline
* Jenkins and Maven integration
* Jenkins and Nexus integration
* Nexus Repository
* Artifact deployment
* CI/CD fundamentals

---

# 🧑‍💻 Author

**Ajay Pokharel**

GitHub:

https://github.com/iamajaypokharel

---

# ⭐ Project Workflow

```text
Developer
    │
    │ git push
    ▼
GitHub
    │
    │ webhook / Jenkins trigger
    ▼
Jenkins
    │
    ├── Checkout
    ├── Compile
    ├── Test
    ├── Package
    └── Deploy
            │
            ▼
       Nexus Repository
            │
            ▼
       Maven Artifact
```

---

## 📚 Conclusion

This project demonstrates a basic DevOps CI/CD workflow where source code is stored in GitHub, Jenkins automatically builds the Maven project, and Nexus Repository stores the generated Maven artifact.

It provides a simple foundation for learning **Maven, Jenkins, Nexus, artifact management, and CI/CD automation**.
