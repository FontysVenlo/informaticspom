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
    <version>0.8</version>
</parent>
```

## Properties

The following properties are available

| Property | Default value | Description |
| --- | --- | --- |
| project.build.sourceEncoding | UTF-8 | Encoding used for source code |
| java.release | 17 | The java version used |
| mockito.core.version | 4.3.1 | The mockito version |
| junit.jupiter.version | 5.8.2 | The JUnit 5 (Jupiter) version |
| assertj.core.version | 3.22.0 | The assertj version |
| jacoco.version | 0.8.7 | The Java Code Coverage version |
| jacocoArgLine | | Optional arguments that get passed to JaCoCo |
| argLine | | Optional arguments that get passed to maven surefire |
| surefire.opens | | Optional arguments that get passed to maven surefire plugin to open modules |
| maven.surefire.version | 3.0.0-M6 | Maven surefire version |
| maven.compiler.release | ${java.release} | The compiler release version used by maven |
| exec.main | | The main class that should be set in the JAR by maven compiler |

## Profiles

**Default**: The default dependencies (no JavaFX and doesn't generate code coverage)
**Jacoco**: Same as default, but generates the code coverage report.
    - `mvn verify` will fail when the `jacoco.threshold.line` and `jacoco.threshold.line` thresholds aren't met (default: 0, can be overridden via a property)
**JavaFX**: Includes the JavaFX dependencies
**Release**: Use to
release new version of the `informaticspom`

## Publishing

Releasing is as easy as pushing/merging new changes to this repository, when the build passes all the checks it will automatically release a new version on Maven Central.