# Forked
https://github.com/arturdm/jacoco-android-gradle-plugin

A Gradle plugin that adds fully configured `JacocoReport` tasks for unit tests of each Android application and library project variant.

## Why
In order to generate JaCoCo unit test coverage reports for Android projects you need to create `JacocoReport` tasks and configure them by providing paths to source code, execution data and compiled classes. It can be troublesome since Android projects can have different flavors and build types thus requiring additional paths to be set. This plugin provides those tasks already configured for you.

## Usage
```groovy
plugins {
  id 'com.android.application'
  id 'com.hiya.jacoco-android'
}

jacoco {
  toolVersion = "0.8.4"
}

tasks.withType(Test) {
  jacoco.includeNoLocationClasses = true
}

android {
  ...
  productFlavors {
    free {}
    paid {}
  }
}
```

The above configuration creates a `JacocoReport` task for each variant and an additional `jacocoTestReport` task that runs all of them.
```
jacocoTestPaidDebugUnitTestReport
jacocoTestFreeDebugUnitTestReport
jacocoTestPaidReleaseUnitTestReport
jacocoTestFreeReleaseUnitTestReport
jacocoTestReport
```

The plugin excludes Android generated classes from report generation by default. You can use `jacocoAndroidUnitTestReport` extension to add other exclusion patterns if needed.
```groovy
jacocoAndroidUnitTestReport {
  excludes += ['**/AutoValue_*.*',
              '**/*JavascriptBridge.class']
}
```

You can also toggle report generation by type using the extension.
```groovy
jacocoAndroidUnitTestReport {
  csv.enabled false
  html.enabled true
  xml.enabled true
}
```

By default your report will be in `[root_project]/[project_name]/build/jacoco/`
But you can change the local reporting directory :
```groovy
jacocoAndroidUnitTestReport {
  destination "/path/to/the/new/local/directory/"
}
```

To generate all reports run:
```shell
$ ./gradlew jacocoTestReport
```

Reports for each variant are available at `$buildDir/reports/jacoco` in separate subdirectories, e.g. `build/reports/jacoco/jacocoTestPaidDebugUnitTestReport`.

## Examples
* https://github.com/codecov/example-android
* https://github.com/devinciltd/lib

# Publishing
There are three options for publishing this:
1. `./gradlew publishPluginMavenPublicationToMavenLocal` -- push to mavenLocal.
1. `./gradlew publishPluginMavenPublicationToLocalRepository` -- push to a repository located at `build/repo`
1. `./gradlew publishPluginMavenPublicationToArtifactoryRepository` -- push to Hiya's Artifactory.

## License
```
Copyright 2015 Artur Stępniewski

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
