# Java Parent POM

POM parent, which define a set of plugin configuration for the build lifecycle of a Java project.

> All plugins but the `maven-gpg-plugin` are ENABLED BY DEFAULT, to disable them you must follow the
>[Plugin disabling instructions](#plugin-disabling-instructions) 


## Plugin disabling instructions
You can override the following properties in your `pom.xml` to disable the plugins you need.

Just set the skip property to **true** for the plugin you want to activate.

There are some plugins without the `skip` property, so you can disable them by defining the execution stage as `none`.

Default values:
```xml
<properties>
    <maven.clean.plugin.skip>false</maven.clean.plugin.skip>
    <maven.resources.plugin.skip>false</maven.resources.plugin.skip>
    <maven.compiler.plugin.skip>false</maven.compiler.plugin.skip>
    <maven.surefire.plugin.skip>false</maven.surefire.plugin.skip>
    <maven.jar.plugin.skip>false</maven.jar.plugin.skip>
    <maven.source.plugin.jar.phase>package</maven.source.plugin.jar.phase>
    <maven.javadoc.plugin.skip>false</maven.javadoc.plugin.skip>
    <maven.checkstyle.plugin.skip>false</maven.checkstyle.plugin.skip>
    <jacoco.maven.plugin.skip>false</jacoco.maven.plugin.skip>
    <maven.gpg.plugin.skip>true</maven.gpg.plugin.skip>
    <maven.deploy.plugin.skip>false</maven.deploy.plugin.skip>
    <nexus.staging.plugin.deploy.phase>deploy</nexus.staging.plugin.deploy.phase>
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
    <gpg.passphrase></gpg.passphrase>
</properties>
```
This property references the password for the key pair signed by the plugin `maven-gpg-plugin`. 

It is highly recommended using an environment variable, or storing it in a configuration file outside the project.

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


## [Maven Lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)

Plugin goals executed in the lifecycle of Maven phases.

Some goals are executed **only for specific profiles**, which are defined between brackets (i.e. {*coverage*}).

> mvn clean
- [maven-clean-plugin](https://maven.apache.org/plugins/maven-clean-plugin/)
    - `clean:clean` (default-clean)

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