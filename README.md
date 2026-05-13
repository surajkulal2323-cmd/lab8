# DEVOPS LAB QUICK EXECUTION GUIDE

From uploaded manual 

---

# PROGRAM 1 — JAVA + MAVEN + GRADLE INSTALLATION

## Software Required

* JDK 21
* Maven
* Gradle
* Eclipse IDE

---

## Installation

### Install Java

1. Download JDK:

   * [Oracle Java Download](https://www.oracle.com/in/java/technologies/downloads/?utm_source=chatgpt.com)
2. Install → Next → Next → Finish
3. Copy path:

```txt
C:\Program Files\Java\jdk-21\bin
```

4. Open:

```txt
Environment Variables → Path → New
```

5. Paste Java bin path
6. Create:

```txt
JAVA_HOME = C:\Program Files\Java\jdk-21
```

---

### Verify Java

## Commands

```bash
java -version
```

## Output

```txt
java version "21"
```

---

### Install Maven

1. Download:

   * [Maven Download](https://maven.apache.org/download.cgi?utm_source=chatgpt.com)
2. Extract ZIP
3. Move to:

```txt
C:\Program Files\
```

4. Copy:

```txt
C:\Program Files\apache-maven-x.x.x\bin
```

5. Add to PATH

## Commands

```bash
mvn -v
```

## Output

```txt
Apache Maven x.x.x
```

---

### Install Gradle

1. Download:

   * [Gradle Download](https://gradle.org/releases/?utm_source=chatgpt.com)
2. Extract ZIP
3. Move to:

```txt
C:\Program Files\
```

4. Copy:

```txt
C:\Program Files\gradle-x.x.x\bin
```

5. Add to PATH

## Commands

```bash
gradle -v
```

## Output

```txt
Gradle x.x.x
```

---

# PROGRAM 2 — MAVEN SPRING BOOT PROJECT

## Software Required

* Eclipse IDE
* Maven
* Java 21

---

## Installation

Install Eclipse:

* [Eclipse IDE Download](https://www.eclipse.org/downloads/?utm_source=chatgpt.com)

---

## Run Steps

### Create Project

1. Create folder:

```txt
D:\FirstProgram
```

2. Open:

* [Spring Initializr](https://start.spring.io/?utm_source=chatgpt.com)

3. Select:

```txt
Project: Maven
Language: Java
```

4. Click:

```txt
GENERATE
```

5. Extract ZIP into:

```txt
D:\FirstProgram
```

---

### Import into Eclipse

1. Open Eclipse
2. Click:

```txt
File → Import
```

3. Search:

```txt
Existing Maven Projects
```

4. Browse extracted folder
5. Finish

---

## Code

### Open `pom.xml`

Paste inside `<plugins>`:

```xml
<plugin>
 <groupId>org.apache.maven.plugins</groupId>
 <artifactId>maven-surefire-plugin</artifactId>
 <configuration>
 <argLine>
 -javaagent:${settings.localRepository}/org/mockito/mockito-core/${mockito.version}/mockito-core-${mockito.version}.jar
 -Xshare:off
 </argLine>
 </configuration>
</plugin>
```

---

### Create File

```txt
src/main/java/.../HelloController.java
```

### Paste Code

```java
package com.example.myfirstspringbootapp;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

@GetMapping("/")
public String sayHello() {
return "Hello, World!";
}

}
```

---

## Commands

### Create JAR

Right Click Project →

```txt
Run As → Maven Build
```

Run:

```txt
clean install
```

---

### Run JAR

Open terminal in project folder:

```bash
java -jar target\myjar.jar
```

---

## Output

Open browser:

```txt
http://localhost:8080
```

Expected:

```txt
Hello, World!
```

---

## Error Fixes

### Port Error

```bash
netstat -ano | findstr 8080
```

### Maven Error

```txt
Right Click Project → Maven → Update Project
```

### Build Error

```txt
File → Restart Eclipse
```

---

# PROGRAM 3 — GRADLE PROJECT

## Commands

```bash
mkdir pgm3
cd pgm3
gradle init
```

Choose:

```txt
1 → Application
3 → Groovy
Java Version → 21
Project Name → gradletest
1 → Single Application
1 → Kotlin DSL
```

---

## Code

### Open `build.gradle`

Paste:

```gradle
task hello {
    doLast {
        println 'Hello, Gradle!'
    }
}
```

---

## Commands

```bash
gradle hello
```

---

## Output

```txt
Hello, Gradle!
```

---

### Run Project

```bash
gradlew run
```

---

# PROGRAM 4 — MAVEN TO GRADLE MIGRATION

## Commands

```bash
gradle init
```

Select:

```txt
yes → migrate Maven to Gradle
2 → Groovy
yes → API Generator
```

---

### Open `build.gradle`

## Code

```gradle
plugins {
 id 'java-library'
 id 'maven-publish'
 id 'application'
}

application {
 mainClass = 'com.ppg4.ppg4.App'
}

repositories {
 mavenLocal()
 maven {
 url = uri('https://repo.maven.apache.org/maven2/')
 }
}

dependencies {
 testImplementation libs.junit.junit
}

group = 'com.ppg4'
version = '0.0.1-SNAPSHOT'
description = 'ppg4'

java.sourceCompatibility = JavaVersion.VERSION_1_8

publishing {
 publications {
  maven(MavenPublication) {
   from(components.java)
  }
 }
}

tasks.withType(JavaCompile) {
 options.encoding = 'UTF-8'
}

tasks.withType(Javadoc) {
 options.encoding = 'UTF-8'
}
```

---

## Commands

```bash
gradle clean build
gradle run
```

---

## Output

```txt
Hello World! Welcome to pgm4
```

---

# PROGRAM 5 — JENKINS INSTALLATION

## Software Required

* Java 17/21
* Jenkins

---

## Installation

### Download Jenkins

* [Jenkins Download](https://www.jenkins.io/download/?utm_source=chatgpt.com)

### Install

1. Open MSI
2. Select:

```txt
Run service as Local System
```

3. Port:

```txt
8080
```

4. Finish installation

---

## Commands

```bash
java -version
```

---

## Run Steps

Open:

```txt
http://localhost:8080
```

Password location:

```txt
C:\ProgramData\Jenkins\.jenkins\secrets\initialAdminPassword
```

Copy password → Paste in browser

Click:

```txt
Install Suggested Plugins
```

Create admin user

---

## Output

```txt
Jenkins is ready!
```

---

## Error Fixes

### Jenkins not opening

Restart service:

```bash
services.msc
```

Restart:

```txt
Jenkins
```

### Port Busy

Change Jenkins port from:

```txt
8080 → 8081
```

---

# PROGRAM 6 — JENKINS CI PIPELINE

## Run Steps

### Install Maven Plugin

1. Jenkins Dashboard
2. Manage Jenkins
3. Plugins
4. Search:

```txt
Maven Integration Plugin
```

5. Install

---

### Create Freestyle Project

1. New Item
2. Enter Name
3. Select:

```txt
Freestyle Project
```

---

### Configure Build

1. Scroll to:

```txt
Build
```

2. Add Build Step →

```txt
Invoke top-level Maven targets
```

3. Maven Version:

```txt
MAVEN_HOME
```

4. Goals:

```txt
clean install
```

5. Advanced →
   Add:

```txt
Path to pom.xml
```

Example:

```txt
C:\Users\...\pom.xml
```

6. Save

---

## Run Project

Click:

```txt
Build Now
```

---

## Output

```txt
BUILD SUCCESS
```

Check:

```txt
Console Output
```

---

## Error Fixes

### Maven Not Found

Go:

```txt
Manage Jenkins → Tools → Maven
```

Set Maven path correctly.

### Build Failed

Check:

```txt
pom.xml path
```

---

Source manual: 
# PROGRAM 7 — ANSIBLE BASIC PLAYBOOK

From uploaded DevOps manual 

---

# PROGRAM 7 — ANSIBLE PLAYBOOK

## Software Required

* Python
* Ansible
* VS Code / Notepad

---

## Installation

### Install Python

* [Python Download](https://www.python.org/downloads/?utm_source=chatgpt.com)

During install:

```txt id="zex35d"
✔ Add Python to PATH
```

---

### Install Ansible (Windows)

Open CMD:

## Commands

```bash id="d2qmyr"
pip install ansible
```

Verify:

```bash id="7jg1jp"
ansible --version
```

---

## Create Files

Create folder:

```bash id="vovv1p"
mkdir ansible-pgm
cd ansible-pgm
```

---

### Create `inventory.ini`

## Code

```ini id="l4vowv"
[local]
localhost ansible_connection=local
```

---

### Create `playbook.yml`

## Code

```yaml id="c2u8iq"
---
- name: My First Playbook
  hosts: local
  tasks:

    - name: Create File
      file:
        path: hello.txt
        state: touch

    - name: Write Message
      copy:
        content: "Hello from Ansible"
        dest: hello.txt
```

---

## Run Steps

## Commands

```bash id="xfm86d"
ansible-playbook -i inventory.ini playbook.yml
```

---

## Output

```txt id="k39ibq"
PLAY RECAP
localhost : ok=2 changed=2 failed=0
```

File created:

```txt id="d26y1o"
hello.txt
```

Content:

```txt id="z1u4u6"
Hello from Ansible
```

---

## Error Fixes

### ansible not recognized

```bash id="s4y1tz"
pip install ansible
```

Restart CMD.

---

### YAML Error

Check spacing:

```txt id="09f4x9"
Use spaces only
No tabs
```

---

# PROGRAM 8 — JENKINS + ANSIBLE DEPLOYMENT

## Software Required

* Jenkins
* Maven
* Ansible

---

## Run Steps

### Open Jenkins

```txt id="ozr1t6"
http://localhost:8080
```

---

### Create New Project

1. Click:

```txt id="eon7fu"
New Item
```

2. Enter name:

```txt id="0rmpjt"
AnsibleDeploy
```

3. Select:

```txt id="p2vx6f"
Freestyle Project
```

4. OK

---

### Configure Maven Build

Scroll → Build

Click:

```txt id="0v0p2o"
Add Build Step
```

Choose:

```txt id="1y9s6g"
Invoke top-level Maven targets
```

Goals:

```txt id="h2zcy5"
clean install
```

POM path:

```txt id="w5rtbr"
C:\path\to\pom.xml
```

---

### Add Ansible Step

Click:

```txt id="26zj59"
Add Build Step
```

Choose:

```txt id="tlf5yv"
Execute Windows batch command
```

Paste:

## Code

```bat id="b3zy02"
ansible-playbook -i inventory.ini playbook.yml
```

---

## Run Project

Click:

```txt id="04m1ua"
Build Now
```

---

## Output

```txt id="hdd9rv"
BUILD SUCCESS
```

Console output shows:

```txt id="2txcjm"
PLAY RECAP
```

---

## Error Fixes

### ansible command failed

Use full path:

```bat id="m8z9qv"
C:\Python\Scripts\ansible-playbook.exe -i inventory.ini playbook.yml
```

---

### Jenkins permission error

Run Jenkins as Administrator.

---

# PROGRAM 9 — AZURE DEVOPS ACCOUNT SETUP

## Software Required

* Microsoft Account
* Browser

---

## Installation

Open:

* [Azure DevOps](https://dev.azure.com/?utm_source=chatgpt.com)

---

## Run Steps

1. Click:

```txt id="y49nzb"
Start Free
```

2. Login with Microsoft account

3. Create Organization

4. Create Project

Project settings:

```txt id="srt0jj"
Visibility → Private
Version Control → Git
Work Item Process → Agile
```

5. Click:

```txt id="4v6kpn"
Create Project
```

---

## Output

```txt id="8rghr4"
Azure DevOps Dashboard Opened
```

---

## Error Fixes

### Microsoft login failed

Use:

```txt id="w8y5k7"
Outlook account
```

---

### Organization already exists

Use different organization name.

---

# PROGRAM 10 — AZURE BUILD PIPELINE

## Software Required

* Azure DevOps
* GitHub account
* Maven/Gradle project

---

## Run Steps

### Open Azure DevOps

Go:

```txt id="30daxg"
Pipelines → Create Pipeline
```

---

### Select Repository

Choose:

```txt id="f03r66"
GitHub
```

Authorize GitHub.

Select repository.

---

### Configure Pipeline

Choose:

```txt id="6mcl2u"
Maven
```

OR

```txt id="k5m84w"
Gradle
```

---

### Azure YAML Example

## Code

```yaml id="4i7d12"
trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- task: Maven@4
  inputs:
    mavenPomFile: 'pom.xml'
    goals: 'clean install'
```

---

## Run Steps

Click:

```txt id="lkl1h7"
Save and Run
```

---

## Output

```txt id="ywyylu"
Build Succeeded
```

---

## Error Fixes

### pom.xml not found

Check:

```txt id="2k81w6"
pom.xml in root folder
```

---

### GitHub authorization failed

Reconnect GitHub account.

---

### Build failed

Run locally first:

```bash id="5q44n4"
mvn clean install
```

---

Source: uploaded DevOps lab manual 
