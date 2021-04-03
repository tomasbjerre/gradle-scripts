# Gradle Scripts

[![Build Status](https://travis-ci.org/tomasbjerre/gradle-scripts.svg?branch=master)](https://travis-ci.org/tomasbjerre/gradle-scripts)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/se.bjurr.gradle/gradle-scripts/badge.svg)](https://search.maven.org/artifact/se.bjurr.gradle/gradle-scripts)

This is a gradle script, written in a highly configurable way, much like I've [blogged about in Jenkins](https://www.jenkins.io/blog/2020/10/21/a-sustainable-pattern-with-shared-library/). It contains a bunch of features that I use in my Gradle projects.

Example usage can be found in:

 * [Git Changelog Lib](https://github.com/tomasbjerre/git-changelog-lib/blob/master/build.gradle)
 * [Violations Lib](https://github.com/tomasbjerre/violations-lib/blob/master/build.gradle)

# How does it work?

It packages a `jar`. Uploads it to a Maven repository. Users can add the `jar` to their classpath and apply the `main.gradle` script from that `jar`.

The behaviour of the script is highly configurable by supplying a `project.ext.buildConfig`. The given config will be merged with the [defaultConfig](src/main/resources/main.gradle).

```groovy
apply plugin: 'java'

buildscript {
 repositories {
  mavenCentral()
  mavenLocal()
 }
 dependencies {
  classpath 'se.bjurr.gradle:gradle-scripts:2.+'
 }
}
project.ext.buildConfig = [
  // Your can supply a given config here, a subset of defaultConfig.
]
apply from: project.buildscript.classLoader.getResource('main.gradle').toURI()
```

# Features

Missing something? Pull requests are welcome =)

All configuration options are documented in the [defaultConfig](src/main/resources/main.gradle).

<!-- start default config -->
```groovy

def defaultConfig = [
  /**
   * "DEFAULT" - Works for most Java projects.
   * "VIOLATIONS" - Much like Java projects, but with some common behavior of my violations lib and its tools.
   * "GRADLE" - A Gradle plugin.
   */
  repoType: "DEFAULT",
  staticCodeAnalysis: [
    /**
    * Maximum number of violations to accept from static code analysis.
    */
    maxViolations: 9999,
  ],
  publishing: [
    website: 'https://github.com/tomasbjerre/' + project.name,
    vcsUrl: 'https://github.com/tomasbjerre/'+project.name,
    licenseName: 'The Apache Software License, Version 2.0',
    licenseUrl: 'http://www.apache.org/licenses/LICENSE-2.0.txt',
    developerId: 'tomasbjerre',
    developerName: 'Tomas Bjerre',
    developerEmail: 'tomas.bjerre85@gmail.com',
    /**
    * If using PasswordCredentials and repository name "nexus", you will
    * need to supply username/password properties named "nexusUsername"
    * and "nexusPassword". Perhaps in ~/.gradle/gradle.properties
    */
    mavenRepositoryName: "nexus",
    mavenRepositoryUrl: 'https://oss.sonatype.org/service/local/staging/deploy/maven2/',
    /**
     * true, if you want the repository in Nexus to be closed and pomoted to Release.
     */
    nexusCloseAndRelease: true,
    /**
    * If using signing you will need to supply signing properties.
    * Perhaps in ~/.gradle/gradle.properties
    * https://docs.gradle.org/current/userguide/signing_plugin.html
    */
    sign: true,
    /**
    * If not empty: creates a fat jar with relocated packages.
    */
    relocate: [], // Example: [ "com.google" ]
  ],
  gradlePlugin: [
    tags: []
  ],
  changelog: [
    enabled: true,
    githubTokenEnvVariableName: 'GITHUB_OAUTH2TOKEN',
    githubOrganization: 'tomasbjerre',
    jiraIssuePattern: "\\bJENKINS-([0-9]+)\\b",
    ignoreCommitsIfMessageMatches: "^\\[maven-release-plugin\\].*|^\\[Gradle Release Plugin\\].*|^Merge.*|.*\\[GRADLE SCRIPT\\].*"
  ],
]

```
<!-- end default config -->
## Publishing releases

It can:

 * Step version
 * Package release
 * Sign release
 * Publish release to repository (like Nexus or Maven Central)
 * Create a changelog
 * Commit/tag/push to Git

With:

```sh
./gradlew releasePatch
./gradlew releaseMinor
./gradlew releaseMajor
```

## Static code analysis

It has `Spotbugs` configured in combination with [Violations Gradle Plugin](https://github.com/tomasbjerre/violations-gradle-plugin). Any violations will be printed nicely in the build log. And build can optionally be failed based on number of violations.

## Other features

 * Automated changelog
 * Code formatting, with Google Java Format.
 * Shadowing, producing fat-jar
 * Signing artifacts with PGP
 * ...

# Requirements

Tested with Gradle 6.8.3.

```sh
./gradlew wrapper --gradle-version=6.8.3 --distribution-type=bin
```

# Developer instructions

You can fiddle with the script localy by installning it with:

```sh
./gradlew publishToMavenLocal -Pversion=latest-SNAPSHOT
```
