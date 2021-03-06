[![Maven Verify](https://github.com/malkomich/java-parent-pom/workflows/Maven%20Verify/badge.svg?branch=master)](https://github.com/malkomich/java-parent-pom/actions?query=workflow%3A%22Maven+Verify%22)
[![Maven Deploy](https://github.com/malkomich/java-parent-pom/workflows/Maven%20Deploy/badge.svg?branch=master)](https://github.com/malkomich/java-parent-pom/actions?query=workflow%3A%22Maven+Deploy%22)
[![Maven Release](https://github.com/malkomich/java-parent-pom/workflows/Release/badge.svg)](https://github.com/malkomich/java-parent-pom/actions?query=workflow%3ARelease)
[![Sonatype Nexus (Releases)](https://img.shields.io/nexus/r/com.github.malkomich/java-parent-pom?label=RELEASE&nexusVersion=2&server=https%3A%2F%2Foss.sonatype.org&color=informational&logo=java&logoColor=critical)](https://repo1.maven.org/maven2/com/github/malkomich/java-parent-pom/)

# Java Parent POM

POM parent, which define a set of plugin configuration for the build lifecycle of a Java project.

> All plugins but `maven-gpg-plugin` and `nexus-staging-plugin`, are ENABLED BY DEFAULT. 
>
>To disable them you must follow the [Plugin disabling instructions](#plugin-disabling-instructions) 


## Usage
Include this parent in your Maven `pom.xml` file:
```xml
<parent>
    <groupId>%PROJECT.GROUP_ID%</groupId>
    <artifactId>%PROJECT.ARTIFACT_ID%</artifactId>
    <version>%PROJECT.VERSION%</version>
</parent>
```


## Plugin disabling instructions
You can override the following properties in your `pom.xml` to disable the plugins you need.

Just set the skip property to **true** for the plugin you want to activate.

There are some plugins without the `skip` property, so you can disable them by defining the execution stage as `none`.

Default values:
```xml
<properties>
    <maven.clean.plugin.skip>%PLUGINS.MAVEN_CLEAN.SKIP%</maven.clean.plugin.skip>
    <maven.scm.plugin.phase>%PLUGINS.MAVEN_SCM.PHASE%</maven.scm.plugin.phase>
    <maven.resources.plugin.skip>%PLUGINS.MAVEN_RESOURCES.SKIP%</maven.resources.plugin.skip>
    <maven.compiler.plugin.skip>%PLUGINS.MAVEN_COMPILER.SKIP%</maven.compiler.plugin.skip>
    <maven.surefire.plugin.skip>%PLUGINS.MAVEN_SUREFIRE.SKIP%</maven.surefire.plugin.skip>
    <maven.jar.plugin.skip>%PLUGINS.MAVEN_JAR.SKIP%</maven.jar.plugin.skip>
    <maven.source.plugin.jar.phase>%PLUGINS.MAVEN_SOURCE.PHASE%</maven.source.plugin.jar.phase>
    <maven.javadoc.plugin.skip>%PLUGINS.MAVEN_JAVADOC.SKIP%</maven.javadoc.plugin.skip>
    <maven.checkstyle.plugin.skip>%PLUGINS.MAVEN_CHECKSTYLE.SKIP%</maven.checkstyle.plugin.skip>
    <jacoco.maven.plugin.skip>%PLUGINS.JACOCO.SKIP%</jacoco.maven.plugin.skip>
    <maven.gpg.plugin.skip>%PLUGINS.MAVEN_GPG.SKIP%</maven.gpg.plugin.skip>
    <maven.deploy.plugin.skip>%PLUGINS.MAVEN_DEPLOY.SKIP%</maven.deploy.plugin.skip>
    <nexus.staging.plugin.deploy.phase>%PLUGINS.NEXUS_STAGING.PHASE%</nexus.staging.plugin.deploy.phase>
</properties>
```
You can also disable some plugins, and enable them just for specific profiles, for example:
```xml
<profiles>
    <!-- GPG Signature on release -->
    <profile>
        <id>release-sign-artifacts</id>
        <activation>
            <property>
                <name>performRelease</name>
                <value>true</value>
            </property>
        </activation>
        <properties>
            <maven.gpg.plugin.skip>false</maven.gpg.plugin.skip>
        </properties>
    </profile>

    <profile>
        <id>coverage</id>
        <properties>
            <maven.surefire.plugin.skip>false</maven.surefire.plugin.skip>
            <jacoco.maven.plugin.skip>false</jacoco.maven.plugin.skip>
        </properties>
    </profile>
</profiles>
```


## Plugin configuration instructions

Some plugins are configurable, based on your needs.

You just have to override the following properties and set your custom values.

- [maven-clean-plugin](https://maven.apache.org/plugins/maven-clean-plugin/)

Plugin to clean build files.

- [maven-scm-plugin](https://maven.apache.org/scm/maven-scm-plugin/)

Plugin to validate SCM configuration.
```xml
<properties>
    <maven.scm.plugin.pushChanges>true</maven.scm.plugin.pushChanges>
</properties>
```

- [maven-compiler-plugin](https://maven.apache.org/plugins/maven-compiler-plugin/)

Plugin to compile your Java project.
```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <java.version>1.8</java.version>
</properties>
```

- [maven-surefire-plugin](https://maven.apache.org/surefire/maven-surefire-plugin/)

Plugin to generate a report for project test.
```xml
<properties>
    <maven.surefire.plugin.failIfNoTests>false</maven.surefire.plugin.failIfNoTests>
</properties>
```

- [maven-javadoc-plugin](https://maven.apache.org/plugins/maven-javadoc-plugin/)

Plugin to attach generated javadoc to the built jar.
```xml
<properties>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
</properties>
```

- [maven-checkstyle-plugin](https://maven.apache.org/plugins/maven-checkstyle-plugin/)

Plugin to analyze the code following a set of code style rules, defined in a given file.
```xml
<properties>
    <checkstyle.config.location>google_checks.xml</checkstyle.config.location>
    <checkstyle.fail.violations>true</checkstyle.fail.violations>
    <checkstyle.log.violations>true</checkstyle.log.violations>
    <checkstyle.max.violations>0</checkstyle.max.violations>
    <checkstyle.violation.severity>error</checkstyle.violation.severity>
</properties>
```

- [jacoco-maven-plugin](https://www.eclemma.org/jacoco/trunk/doc/maven.html)

Plugin to analyze the test coverage and generates a coverage report.
```xml
<properties>
    <jacoco.plugin.bundle.instruction.coveredratio>0%</jacoco.plugin.bundle.instruction.coveredratio>
    <jacoco.plugin.bundle.line.coveredratio>0%</jacoco.plugin.bundle.line.coveredratio>
    <jacoco.plugin.bundle.branch.coveredratio>0%</jacoco.plugin.bundle.branch.coveredratio>
    <jacoco.plugin.bundle.complexity.coveredratio>0%</jacoco.plugin.bundle.complexity.coveredratio>
    <jacoco.plugin.bundle.method.coveredratio>0%</jacoco.plugin.bundle.method.coveredratio>
    <jacoco.plugin.bundle.class.missedCount>0</jacoco.plugin.bundle.class.missedCount>
</properties>
```

You can add exclusions on any classes or packages you want to skip, defining them in the pluginManagement section:
```xml
<build>
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.jacoco</groupId>
                <artifactId>jacoco-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>packageName/subPackageName/**/*</exclude>
                        <exclude>packageName.subPackageName.*</exclude>
                        <exclude>**/classPrefix*.class</exclude>
                        <exclude>**/*classSuffix.class</exclude>
                        <exclude>**/intermediatePackageName/**/*</exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </pluginManagement>
</build>
```

- [maven-gpg-plugin](https://maven.apache.org/plugins/maven-gpg-plugin/)

Plugin to sign the built artifacts.

You must first [Install GNU PG](https://www.gnupg.org/download/), and generate the key pair, to use this plugin.

- [maven-deploy-plugin](http://maven.apache.org/plugins/maven-deploy-plugin/)

Plugin for pushing artifacts to a remote repository.

- [nexus-staging-maven-plugin](https://github.com/sonatype/nexus-maven-plugins/tree/master/staging/maven-plugin)

Plugin to control Nexus Staging workflow.

To use this plugin, you must define the sonatype credentials in your global configuration:
```xml
<settings>
    <servers>
        <server>
            <id>ossrh</id>
            <username>your-jira-id</username>
            <password>your-jira-pwd</password>
        </server>
    </servers>
</settings>
```
Moreover, in your project's configuration, you have to define the source control management (SCM) properties,
and the distribution management configuration:
```xml
<project>
    <scm>
        <connection>scm:git:git://github.com/malkomich/java-parent-pom.git</connection>
        <developerConnection>scm:git:git@github.com:malkomich/java-parent-pom.git</developerConnection>
        <url>https://github.com/malkomich/java-parent-pom</url>
        <tag>HEAD</tag>
    </scm>
    
    <distributionManagement>
        <snapshotRepository>
            <id>ossrh</id>
            <url>https://oss.sonatype.org/content/repositories/snapshots</url>
        </snapshotRepository>
        <repository>
            <id>ossrh</id>
            <url>https://oss.sonatype.org/service/local/staging/deploy/maven2/</url>
        </repository>
    </distributionManagement>
</project>
```

- [maven-release-plugin](https://maven.apache.org/maven-release/maven-release-plugin/)

Plugin to release the project artifacts.
```xml
<properties>
    <maven.release.plugin.tagNameFormat>v@{project.version}</maven.release.plugin.tagNameFormat>
    <maven.release.plugin.releaseProfiles>ossrh</maven.release.plugin.releaseProfiles>
    <maven.release.plugin.pushChanges>true</maven.release.plugin.pushChanges>
</properties>
```

The steps to execute a release with this plugins are:
1. `mvn release:prepare`:
    - Check that there are no uncommitted changes in the sources
    - Check that there are no SNAPSHOT dependencies
    - Change the version in the POMs from x-SNAPSHOT to a new version (you will be prompted for the versions to use)
    - Transform the SCM information in the POM to include the final destination of the tag
    - Run the project tests against the modified POMs to confirm everything is in working order
    - Commit the modified POMs
    - Tag the code in the SCM with a version name (this will be prompted for)
    - Bump the version in the POMs to a new value y-SNAPSHOT (these values will also be prompted for)
    - Commit the modified POMs
2. `mvn release:perform`:
    - Checkout from an SCM URL with optional tag
    - Run the predefined Maven goals to release the project (by default, deploy site-deploy)

- [versions-maven-plugin](https://www.mojohaus.org/versions-maven-plugin/)

Plugin to manage your pom.xml versions, including the current artifact versions,
parent or dependencies/plugins versions, and even the tag for the SCM configuration.

- [maven-help-plugin](https://maven.apache.org/plugins/maven-help-plugin/)

Plugin to get relative information about the project configuration.


## [Maven Lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)

Plugin goals executed in the lifecycle of Maven phases.

Some goals are executed **only for specific profiles**, which are defined between brackets (i.e. {*coverage*}).

> mvn clean
- [maven-clean-plugin](https://maven.apache.org/plugins/maven-clean-plugin/)
    - `clean:clean` (default-clean)

> mvn validate
- [maven-scm-plugin](https://maven.apache.org/scm/maven-scm-plugin/)
    - `scm:validate` (validate-scm)

> mvn process-resources
- [maven-resources-plugin](http://maven.apache.org/plugins/maven-resources-plugin/)
    - `resources:resources` (default-resources)

> mvn compile
- [maven-compiler-plugin](https://maven.apache.org/plugins/maven-compiler-plugin/)
    - `compiler:compile` (default-compile)

> mvn process-test-resources
- [maven-resources-plugin](http://maven.apache.org/plugins/maven-resources-plugin/)
    - `resources:testResources` (default-testResources)

> mvn test-compile
- [maven-compiler-plugin](https://maven.apache.org/plugins/maven-compiler-plugin/)
    - `compiler:testCompile` (default-testCompile)
- {*coverage*} [jacoco-maven-plugin](https://www.eclemma.org/jacoco/trunk/doc/maven.html)
    - `jacoco:prepare-agent` (coverage-initialize)

> mvn test
- {*coverage*} [maven-surefire-plugin](https://maven.apache.org/surefire/maven-surefire-plugin/)
    - `surefire:test` (default-test)

> mvn package
- [maven-jar-plugin](http://maven.apache.org/plugins/maven-jar-plugin/)
    - `jar:jar` (default-jar)
- [maven-source-plugin](http://maven.apache.org/plugins/maven-source-plugin/)
    - `source:jar` (attach-sources)
- [maven-javadoc-plugin](https://maven.apache.org/plugins/maven-javadoc-plugin/)
    - `javadoc:jar` (attach-javadoc)

> mvn verify
- [maven-checkstyle-plugin](https://maven.apache.org/plugins/maven-checkstyle-plugin/)
    - `checkstyle:check` (checkstyle)
- {*coverage*} [jacoco-maven-plugin](https://www.eclemma.org/jacoco/trunk/doc/maven.html)
    - `jacoco:report` (coverage-report)
- {*coverage*} [jacoco-maven-plugin](https://www.eclemma.org/jacoco/trunk/doc/maven.html)
    - `jacoco:check` (coverage-check)
- {*release-sign-artifacts*} [maven-gpg-plugin](https://maven.apache.org/plugins/maven-gpg-plugin/)
    - `gpg:sign` (sign-artifacts)

> mvn deploy
- [maven-deploy-plugin](http://maven.apache.org/plugins/maven-deploy-plugin/)
    - `deploy:deploy` (default-deploy)
- [nexus-staging-maven-plugin](https://github.com/sonatype/nexus-maven-plugins/tree/master/staging/maven-plugin)
    - `nexus-staging-maven-plugin:deploy` (injected-nexus-deploy)