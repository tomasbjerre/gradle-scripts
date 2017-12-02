# Gradle Scripts

This is a collection of gradle scripts that I use in other projects.


Other projects can apply these scripts like this.

```
buildscript {
 repositories {
  jcenter()
  maven { url "https://jitpack.io" }
 }
 dependencies {
  classpath 'com.github.tomasbjerre:gradle-scripts:master-SNAPSHOT'
 }
 dependencies {
  ant.unjar src: configurations.classpath.singleFile, dest: 'build/gradle'
 }
}

afterEvaluate { project ->
 apply from: 'build/gradle/java.gradle'
 apply from: 'build/gradle/changelog.gradle'
 apply from: 'build/gradle/release.gradle'
}
```

Jitpack can be slow but you can increase the timeouts like this:
```
./gradlew build -Dhttp.socketTimeout=60000 -Dhttp.connectionTimeout=60000
```

