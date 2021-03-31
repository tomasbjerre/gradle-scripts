# Gradle Scripts

[![Build Status](https://travis-ci.org/tomasbjerre/gradle-scripts.svg?branch=master)](https://travis-ci.org/tomasbjerre/gradle-scripts)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/se.bjurr.gradle/gradle-scripts/badge.svg)](https://maven-badges.herokuapp.com/maven-central/se.bjurr.violations/gradle-scripts)

This is how I share Gradle code between my projects. It is written in a highly configurable way, much like:
https://www.jenkins.io/blog/2020/10/21/a-sustainable-pattern-with-shared-library/

Example usage can be found in:

 * https://github.com/tomasbjerre/violations-lib
 * https://github.com/tomasbjerre/git-changelog-lib
 * https://github.com/tomasbjerre/violation-comments-to-github-gradle-plugin

Applying them via a Maven repository means:

 * The code will be cached. Not downloaded for every build, making them faster, and builds can be performed when offline.
 * Versions can be released and managed in branches.

It is used by adding the `jar` to classpath and apply the `main.gradle` script from that `jar`. The behaviour can be tweaked by supplying a `project.ext.buildConfig` that will be merged with the [defaultConfig](src/main/resources/main.gradle).

```groovy
apply plugin: 'java'

buildscript {
 repositories {
  mavenCentral()
 }
 dependencies {
  classpath 'com.github.tomasbjerre:gradle-scripts:a.b'
 }
}
project.ext.buildConfig = [
  // Your can supply a given config here and adjust parts of default config in:
  // src/main/resources/main.gradle
]
apply from: project.buildscript.classLoader.getResource('main.gradle').toURI()
```
