Two extensions to Gradle `Test` tasks:
* Unconditionally run all tests (i.e. also cached tests)
* Repeated `Test` executor invocations (does not work with Gradle Enterprise Distributed Testing)

By default, Gradle only runs tests for sources (test-class inputs), that have changed. Sometimes you want to rerun
some or all tests, e.g. when investigating/fixing flaky tests. Running Gradle with the project property
`testRerun` (e.g. `./gradlew test -PtestRerun`) marks the test as "not up-to-date" and all tests matched tests will
be run.

In addition to the above, specifying `-PtestRepetitions=N` repeats `Test` tasks `N` times, which can also be quite
handy when investigating/fixing flaky tests.
*NOTE* Repeating tests does _not_ work with [Gradle Enterprise Distributed Testing](https://docs.gradle.com/enterprise/test-distribution-gradle-plugin/),
as both the distributed-testing plugin and this plugin use the same (undocumented) approach to update the
`testExecutor` field by making the package-protected method `org.gradle.api.tasks.testing.Test.setTestExecuter`
accessible. If you're interested in this functionality in GE's dist-testing, please ping Gradle.

Usage:

```
plugins {
    id("org.caffinitas.gradle.testrerun") version "0.1"
}
```
