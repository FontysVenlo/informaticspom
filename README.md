# informaticspom

The `informaticspom` is used as the parent pom for programming assignments at Fontys Venlo.

The goal of the `POM` is to have a baseline of dependencies that are needed for most assignments.

These include:

1. Test dependencies
    - JUnit
    - AssertJ
    - Mockito
2. CodeCoverage with JaCoCo
3. Static code analyzer
    - CheckStyle
4. JavaFX (with the JavaFX profile)
5. Testing modular JPMS projects

## Usage

The pom can be used by defining it as the parent pom

```
<parent>
    <groupId>io.github.fontysvenlo</groupId>
    <artifactId>informaticspom</artifactId>
    <version>1.4</version>
</parent>
```

## Properties

The following properties are available

| Property | Default value | Description |
| --- | --- | --- |
| project.build.sourceEncoding | UTF-8 | Encoding used for source code |
| java.release | 21 | The java version used |
| mockito.core.version | 5.9.0 | The mockito version |
| junit.jupiter.version | 5.10.1 | The JUnit 5 (Jupiter) version |
| assertj.core.version | 3.25.1 | The assertj version |
| jacoco.version | 0.8.11 | The Java Code Coverage version |
| jacocoArgLine | | Optional arguments that get passed to JaCoCo |
| argLine | | Optional arguments that get passed to maven surefire |
| surefire.opens | | Optional arguments that get passed to maven surefire plugin to open modules |
| surefire.opens.to | ALL-UNNAMED | The modules to open the packages in `surefire.opens` to, see [Modular projects](#modular-projects) |
| slf4j.version | 2.0.17 | The version of `slf4j-nop`, keeps ArchUnit from warning about a missing SLF4J provider |
| maven.surefire.version | 3.2.5 | Maven surefire version |
| maven.compiler.release | ${java.release} | The compiler release version used by maven |
| exec.main | | The main class that should be set in the JAR by maven compiler, also used by `mvn exec:java` (override with `-Dexec.main=...`) |

## Profiles

**Default**: The default dependencies (no JavaFX and doesn't generate code coverage)
**Jacoco**: Same as default, but generates the code coverage report.
    - `mvn verify` will fail when the `jacoco.threshold.line` and `jacoco.threshold.branch` thresholds aren't met (default: 0, can be overridden via a property)
**JavaFX**: Includes the JavaFX dependencies
**Release**: Use to
release new version of the `informaticspom`
**intellij-modules**: Activated automatically when IntelliJ imports a project that has a `src/main/java/module-info.java`, see [Modular projects](#modular-projects)

## Modular projects

Open the packages that the tests need reflective access to with `surefire.opens`, and open them to `${surefire.opens.to}`:

```
<surefire.opens>
    --add-opens mymodule/mypackage=${surefire.opens.to}
</surefire.opens>
```

IntelliJ (2026.2) runs the tests of a modular project with the JUnit platform on the module path, but the Jupiter engine on the classpath, which fails with `TestEngine with ID 'junit-jupiter' failed to discover tests`. The `intellij-modules` profile works around this: it gives the engine access to the JUnit platform internals and adds `org.junit.platform.commons` to `surefire.opens.to`. Outside IntelliJ `surefire.opens.to` is just `ALL-UNNAMED`, so Maven runs without extra warnings.

## Publishing

Releasing is as easy as pushing/merging new changes to this repository, when the build passes all the checks it will automatically release a new version on Maven Central.
