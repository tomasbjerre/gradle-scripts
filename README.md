# Gradle Scripts

This is a collection of gradle scripts that I use in other projects.


Other projects can apply these scripts like this. See:

 * https://github.com/tomasbjerre/violations-lib
 * https://github.com/tomasbjerre/git-changelog-lib

```
apply plugin: 'java'

project.ext.tags = ['tag1', 'tag2']
group = 'se.bjurr.x'
description = 'Library for ...'


buildscript {
 repositories {
  maven { url "https://jitpack.io" }
 }
 dependencies {
  classpath 'com.github.tomasbjerre:gradle-scripts:master-SNAPSHOT'
 }
}
apply from: project.buildscript.classLoader.getResource('java.gradle').toURI()
apply from: project.buildscript.classLoader.getResource('changelog.gradle').toURI()
apply from: project.buildscript.classLoader.getResource('release.gradle').toURI()
```

Jitpack can be slow but you can increase the timeouts like this:
```
./gradlew build -Dhttp.socketTimeout=60000 -Dhttp.connectionTimeout=60000
```

