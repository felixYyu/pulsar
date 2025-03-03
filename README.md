<!--

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.

-->

![logo](https://pulsar.apache.org/img/pulsar.svg)

[![docker pull](https://img.shields.io/docker/pulls/apachepulsar/pulsar-all.svg)](https://hub.docker.com/r/apachepulsar/pulsar)
[![contributors](https://img.shields.io/github/contributors-anon/apache/pulsar)](https://github.com/apache/pulsar/graphs/contributors)
[![last commit](https://img.shields.io/github/last-commit/apache/pulsar)](https://github.com/apache/pulsar/commits/master)
[![release](https://img.shields.io/github/v/release/apache/pulsar?sort=semver)](https://pulsar.apache.org/download/)
[![downloads](https://img.shields.io/github/downloads/apache/pulsar/total)](https://pulsar.apache.org/download/)

Pulsar is a distributed pub-sub messaging platform with a very
flexible messaging model and an intuitive client API.

Learn more about Pulsar at https://pulsar.apache.org

## Main features

- Horizontally scalable (Millions of independent topics and millions
  of messages published per second)
- Strong ordering and consistency guarantees
- Low latency durable storage
- Topic and queue semantics
- Load balancer
- Designed for being deployed as a hosted service:
  - Multi-tenant
  - Authentication
  - Authorization
  - Quotas
  - Support mixing very different workloads
  - Optional hardware isolation
- Keeps track of consumer cursor position
- REST API for provisioning, admin and stats
- Geo replication
- Transparent handling of partitioned topics
- Transparent batching of messages

## Repositories

This repository is the main repository of Apache Pulsar. Pulsar PMC also maintains other repositories for
components in the Pulsar ecosystem, including connectors, adapters, and other language clients.

- [Pulsar Core](https://github.com/apache/pulsar)

### Helm Chart

- [Pulsar Helm Chart](https://github.com/apache/pulsar-helm-chart)

### Ecosystem

- [Pulsar Adapters](https://github.com/apache/pulsar-adapters)

### Clients

- [.NET/C# Client](https://github.com/apache/pulsar-dotpulsar)
- [C++ Client](https://github.com/apache/pulsar-client-cpp)
- [Go Client](https://github.com/apache/pulsar-client-go)
- [NodeJS Client](https://github.com/apache/pulsar-client-node)
- [Python Client](https://github.com/apache/pulsar-client-python)
- [Reactive Java Client](https://github.com/apache/pulsar-client-reactive)

### Dashboard & Management Tools

- [Pulsar Manager](https://github.com/apache/pulsar-manager)

### Website

- [Pulsar Site](https://github.com/apache/pulsar-site)

### CI/CD

- [Pulsar CI](https://github.com/apache/pulsar-test-infra)

### Archived/Halted

- [Pulsar Connectors](https://github.com/apache/pulsar-connectors)
- [Pulsar Translation](https://github.com/apache/pulsar-translation)
- [Pulsar SQL (Pulsar Presto Connector)](https://github.com/apache/pulsar-presto)
- [Ruby Client](https://github.com/apache/pulsar-client-ruby)

## Pulsar Runtime Java Version Recommendation

- pulsar ver > 2.10 and master branch

| Pulsar Components | Java Version  |
| ----------------- | :-----------: |
| Broker            |      17       |
| Functions / IO    |      17       |
| CLI               |      17       |
| Java Client       | 8 or 11 or 17 |

- 2.8 <= pulsar ver <= 2.10

| Pulsar Components | Java Version |
| ----------------- | :----------: |
| Broker            |      11      |
| Functions / IO    |      11      |
| CLI               |   8 or 11    |
| Java Client       |   8 or 11    |

- pulsar ver < 2.8

| Pulsar Components | Java Version |
| ----------------- | :----------: |
| All               |   8 or 11    |

## Build Pulsar

### Requirements

- JDK

  | Pulsar Version | JDK Version |
  | ----------------- | :----------: |
  | master and 2.11 + |   [JDK 17](https://adoptium.net/?variant=openjdk17)    |
  | 2.8 / 2.9 / 2.10  |   [JDK 11](https://adoptium.net/?variant=openjdk11)    |
  | 2.7 - |   [JDK 8](https://adoptium.net/?variant=openjdk8)    |

- Maven 3.6.1+
- zip

> **Note**:
>
> This project includes a [Maven Wrapper](https://maven.apache.org/wrapper/) that can be used instead of a system-installed Maven.
> Use it by replacing `mvn` by `./mvnw` on Linux and `mvnw.cmd` on Windows in the commands below.    
>
> It's better to use CMD rather than Powershell on Windows. Because maven will activate the `windows` profile which runs `rename-netty-native-libs.cmd`.

### Build
Compile and install:

```bash
$ mvn install -DskipTests
```

Compile and install individual module

```bash
$ mvn -pl module-name (e.g: pulsar-broker) install -DskipTests
```

### Minimal build (This skips most of external connectors and tiered storage handlers)

```
mvn install -Pcore-modules,-main -DskipTests
```

Run Unit Tests:

```bash
$ mvn test
```

Run Individual Unit Test:

```bash
$ mvn -pl module-name (e.g: pulsar-client) test -Dtest=unit-test-name (e.g: ConsumerBuilderImplTest)
```

Run Selected Test packages:

```bash
$ mvn test -pl module-name (for example, pulsar-broker) -Dinclude=org/apache/pulsar/**/*.java
```

Start standalone Pulsar service:

```bash
$ bin/pulsar standalone
```

Check https://pulsar.apache.org for documentation and examples.

## Build custom docker images

Docker images must be built with Java 8 for `branch-2.7` or previous branches because of
[issue 8445](https://github.com/apache/pulsar/issues/8445).
Java 11 is the recommended JDK version in `branch-2.8`, `branch-2.9` and `branch-2.10`.
Java 17 is the recommended JDK version in `master`.

This builds the docker images `apachepulsar/pulsar-all:latest` and `apachepulsar/pulsar:latest`.

```bash
mvn clean install -DskipTests
mvn package -Pdocker,-main -am -pl docker/pulsar-all -DskipTests
```

After the images are built, they can be tagged and pushed to your custom repository.
Here's an example of a bash script that tags the docker images with the current version and git revision and
pushes them to `localhost:32000/apachepulsar`.

```bash
image_repo_and_project=localhost:32000/apachepulsar
pulsar_version=$(mvn initialize help:evaluate -Dexpression=project.version -pl . -q -DforceStdout)
gitrev=$(git rev-parse HEAD | colrm 10)
tag="${pulsar_version}-${gitrev}"
echo "Using tag $tag"
docker tag apachepulsar/pulsar-all:latest ${image_repo_and_project}/pulsar-all:$tag
docker push ${image_repo_and_project}/pulsar-all:$tag
docker tag apachepulsar/pulsar:latest ${image_repo_and_project}/pulsar:$tag
docker push ${image_repo_and_project}/pulsar:$tag
```

## Setting up your IDE

Apache Pulsar is using [lombok](https://projectlombok.org/) so you have to ensure your IDE setup with
required plugins.

### IntelliJ

#### Configure Project JDK to Java 17 JDK

1. Open **Project Settings**.

   Click **File** -> **Project Structure** -> **Project Settings** -> **Project**.

2. Select the JDK version.

   From the JDK version drop-down list, select **Download JDK...** or choose an existing recent Java 17 JDK version.

3. In the download dialog, select version **17**. You can pick a version from many vendors. Unless you have a specific preference, choose **Eclipse Temurin (AdoptOpenJDK (Hotspot))**.

#### Configure Java version for Maven in IntelliJ

1. Open Maven Importing Settings dialog by going to
   **Settings** -> **Build, Execution, Deployment** -> **Build Tools** -> **Maven** -> **Importing**.

2. Choose **Use Project JDK** for **JDK for Importer** setting. This uses the Java 17 JDK for running Maven
   when importing the project to IntelliJ. Some of the configuration in the Maven build is conditional based on
   the JDK version. Incorrect configuration gets chosen when the "JDK for Importer" isn't the same as the "Project JDK".

3. Validate that the JRE setting in **Maven** -> **Runner** dialog is set to **Use Project JDK**.

#### Configure annotation processing in IntelliJ

1. Open Annotation Processors Settings dialog box by going to
   **Settings** -> **Build, Execution, Deployment** -> **Compiler** -> **Annotation Processors**.

2. Select the following buttons:

   1. **Enable annotation processing**
   2. **Obtain processors from project classpath**
   3. Store generated sources relative to: **Module content root**

3. Set the generated source directories to be equal to the Maven directories:

   1. Set "Production sources directory:" to "target/generated-sources/annotations".
   2. Set "Test sources directory:" to "target/generated-test-sources/test-annotations".

4. Click **OK**.

5. Install the lombok plugin in intelliJ.

#### Configure code style

1. Open Code Style Settings dialog box by going to **Settings** -> **Editor** -> **Code Style**.

2. Click on the :gear: symbol -> **Import scheme** -> **IntelliJ IDEA code style XML**

3. Pick the file `${pulsar_dir}/src/idea-code-style.xml`

4. On the dialog box that opens, click **OK**.

5. Ensure the scheme you just created is selected in **Scheme** dropdown then click **OK**.

#### Configure Checkstyle

1. Install the Checkstyle-IDEA plugin.

2. Open Checkstyle Settings dialog box by going to **Settings** -> **Tools** -> **Checkstyle**.

3. Set **Checkstyle version** to **8.37**.

4. Set **Scan scope** to **Only Java sources (including tests)**.

5. Click **+** button in the **Configuration** section to open a dialog to choose the checkfile file.

   1. Enter a **Description**. For example, Pulsar.
   2. Select **Use a local checkstyle file**.
   3. Set **File** to **buildtools/src/main/resources/pulsar/checkstyle.xml**.
   4. Select **Store relative to project location**.
   5. Click **Next** -> **Next** -> **Finish**.

6. Activate the configuration you just added by toggling the corresponding box.

7. Click **OK**.

#### Further configuration in IntelliJ

- When working on the Pulsar core modules in IntelliJ, reduce the number of active projects in IntelliJ to speed up IDE actions and reduce unrelated IDE warnings.

  - In IntelliJ's Maven UI's tree view under "Profiles"
    - Activate "core-modules" Maven profile
    - De-activate "main" Maven profile
    - Run the "Reload All Maven Projects" action from the Maven UI toolbar. You can also find the action by the name in the IntelliJ "Search Everywhere" window that gets activated by pressing the **Shift** key twice.

- Run the "Generate Sources and Update Folders For All Projects" action from the Maven UI toolbar. You can also find the action by the name in the IntelliJ "Search Everywhere" window that gets activated by pressing the **Shift** key twice. Running the action takes about 10 minutes for all projects. This is faster when the "core-modules" profile is the only active profile.

#### IntelliJ usage tips

- In the case of compilation errors with missing Protobuf classes, ensure to run the "Generate Sources and Update Folders For All Projects" action.

- All of the Pulsar source code doesn't compile properly in IntelliJ and there are compilation errors.
  - Use the "core-modules" profile if working on the Pulsar core modules since the source code for those modules can be compiled in IntelliJ.
  - Sometimes it might help to mark a specific project ignored in IntelliJ Maven UI by right-clicking the project name and select **Ignore Projects** from the menu.
  - Currently, it is not always possible to run unit tests directly from the IDE because of the compilation issues. As a workaround, individual test classes can be run by using the `mvn test -Dtest=TestClassName` command.
- The above steps have all been performed, but a test still won't run.
  - In this case, try the following steps:
    1. Close IntelliJ.
    2. Run `mvn clean install -DskipTests` on the command line.
    3. Reopen IntelliJ.
  - If that still doesn't work:
    1. Verify Maven is using a supported version. Currently, the supported version of Maven is specified in the
       <requireMavenVersion> section of the main pom.xml file.
    2. Try "restart and clear caches" in IntelliJ and repeat the above steps to reload projects and generate sources.

### Eclipse

Follow the instructions [here](https://howtodoinjava.com/automation/lombok-eclipse-installation-examples/)
to configure your Eclipse setup.

## Documentation

> **Tip**
>
> For how to make contributions to Pulsar documentation, see [Pulsar Documentation Contribution Guide](https://docs.google.com/document/d/11DTnNPpvcPrebLkMAFcDEIFlD8ARD-k6F-LXoIwdD9Y/edit#).

## Contact

##### Mailing lists

| Name                                                      | Scope                           |                                                       |                                                           |                                                                    |
| :-------------------------------------------------------- | :------------------------------ | :---------------------------------------------------- | :-------------------------------------------------------- | :----------------------------------------------------------------- |
| [users@pulsar.apache.org](mailto:users@pulsar.apache.org) | User-related discussions        | [Subscribe](mailto:users-subscribe@pulsar.apache.org) | [Unsubscribe](mailto:users-unsubscribe@pulsar.apache.org) | [Archives](http://mail-archives.apache.org/mod_mbox/pulsar-users/) |
| [dev@pulsar.apache.org](mailto:dev@pulsar.apache.org)     | Development-related discussions | [Subscribe](mailto:dev-subscribe@pulsar.apache.org)   | [Unsubscribe](mailto:dev-unsubscribe@pulsar.apache.org)   | [Archives](http://mail-archives.apache.org/mod_mbox/pulsar-dev/)   |

##### Slack

Pulsar slack channel at https://apache-pulsar.slack.com/

You can self-register at https://apache-pulsar.herokuapp.com/

##### Report a security vulnerability

To report a vulnerability for Pulsar, contact the [Apache Security Team](https://www.apache.org/security/). When reporting a vulnerability to [security@apache.org](mailto:security@apache.org), you can copy your email to [private@pulsar.apache.org](mailto:private@pulsar.apache.org) to send your report to the Apache Pulsar Project Management Committee. This is a private mailing list.

https://github.com/apache/pulsar/security/policy contains more details.

## License

Licensed under the Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0

## Crypto Notice

This distribution includes cryptographic software. The country in which you currently reside may have restrictions on the import, possession, use, and/or re-export to another country, of encryption software. BEFORE using any encryption software, please check your country's laws, regulations and policies concerning the import, possession, or use, and re-export of encryption software, to see if this is permitted. See <http://www.wassenaar.org/> for more information.

The U.S. Government Department of Commerce, Bureau of Industry and Security (BIS), has classified this software as Export Commodity Control Number (ECCN) 5D002.C.1, which includes information security software using or performing cryptographic functions with asymmetric algorithms. The form and manner of this Apache Software Foundation distribution makes it eligible for export under the License Exception ENC Technology Software Unrestricted (TSU) exception (see the BIS Export Administration Regulations, Section 740.13) for both object code and source code.

The following provides more details on the included cryptographic software: Pulsar uses the SSL library from Bouncy Castle written by http://www.bouncycastle.org.
