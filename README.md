# Gradle Scripts

[![Build Status](https://travis-ci.org/tomasbjerre/gradle-scripts.svg?branch=master)](https://travis-ci.org/tomasbjerre/gradle-scripts)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/se.bjurr.gradle/gradle-scripts/badge.svg)](https://maven-badges.herokuapp.com/maven-central/se.bjurr.violations/gradle-scripts)

This is how I share Gradle code between my projects. It is written in a highly configurable way, much like:
https://www.jenkins.io/blog/2020/10/21/a-sustainable-pattern-with-shared-library/

Example usage can be found in:

 * [Violations Lib](https://github.com/tomasbjerre/violations-lib/blob/master/build.gradle)

# How does it work?

It packages a `jar`. Uploads it to a Maven repository. Users can add the `jar` classpath and apply the `main.gradle` script from that `jar`.

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
  // Your can supply a given config here and adjust parts of default config in:
  // src/main/resources/main.gradle
]
apply from: project.buildscript.classLoader.getResource('main.gradle').toURI()
```

# Features

This script contains a bunch of features that I need in my different projects. But I rewrote it in `2.0.0` with a new pattern that should allow loads of features. Missing something? Pull requests are welcome =)

All configuration options are documented in the [defaultConfig](src/main/resources/main.gradle).

## Version ubmping

With:

```sh
./gradlew bumpPatch
./gradlew bumpMinor
./gradlew bumpMajor
```

## Publishing releases

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
