# Gradle

### Information
Gradle supports two types of DSL: Groovy and Kotlin

gradle.properties file is used to define the variables which can be used in build.gradle(Applicable only for kotlin type DSL)    

For groovy DSL, buildScipt block will configure the things during running the script build.gradle

### --------------------------------  simple task ------------------------------------

```
task hello {
    doFirst {
        print "Hello"
    }

    doLast {
        println ", World"
    }
}
```

### --------------------------------------- Dependent task --------------------------------

```
task hello {
    doLast {
        print "Hello"
    }
}

task world {
    dependsOn hello
    doLast {
        print "World"
    }
}
```
 
 ### --------------------------------------- Adding plugins ------------------------------------

For first time after adding the plugins, it will download the dependent libraries

For community plugin we have to define the full name and version

```
plugins {
    id "java"
    id "org.flywaydb.flyway" version "7.8.1"
}
```

### -------------------------------- Building Java Project ---------------------------------

There are three Java plugins

java --> Base java plugin

java-library --> Extends the java plugin. To build a Java library

application --> Extends the java plugin. To run the java application during gradle build and uses
the distribute plugin to distribute the java application.

By default gradle looks for the source code similar like maven folder structure

Use SourceSets to define the different folder structure for source code

gradle build -i --> Will print more information about the tasks during execution

Dependencies have four configuration scopes: compilation, runtime, test compilation and test runtime

implementation --> Source code compilation and runtime dependencies

testImplementation --> Test code compilation and test runtime dependencies

To list dependencies "gradle -q dependencies" Here -q for quit

To list dependencies for specific configuration "gradle -q dependencies --configuration implementation"

Each configuration has onlyBuiltime and onlyRunrime configuration

```
/*
 * This file was generated by the Gradle 'init' task.
 *
 * This generated file contains a sample Java application project to get you started.
 * For more details take a look at the 'Building Java & JVM projects' chapter in the Gradle
 * User Manual available at https://docs.gradle.org/7.0/userguide/building_java_projects.html
 */

plugins {
    // Apply the application plugin to add support for building a CLI application in Java.
    id 'application'
}

version = "1.0-SNAPSHOT"

repositories {
    // Use Maven Central for resolving dependencies.
    mavenCentral()
}

dependencies {
    // Use JUnit test framework.
    testImplementation 'junit:junit:4.13.1'

    // This dependency is used by the application.
    implementation 'com.google.guava:guava:30.0-jre'
}

application {
    // Define the main class for the application.
    mainClass = 'gradle.App'
}
```

### ------------------- Including Dependency from local file ---------------------------
Dependencies  jar are already downloaded in lib folder locally 

```
dependencies {
    testimplementation files ('lib/junit-4.13.1.jar')
}
```

### ------------------- Including Dependency from local file in common folder ------------------------
Dependencies  jar are already downloaded in lib folder locally 

```
repositories {
    flatDir {
        dirs 'lib'
    }
}

dependencies {
    testImplementation 'junit:junit:4.13.1'
    //or
    //testImplementation group: 'junit', name: 'junit', version: '4.13.1' 
}
```

### ------- Define varibales in build.gradle file  for Groovy DSL using buildScript block ------

```
buildScript {
    ext {
        junit_version = "4.13.1"
    }
}

dependencies {
    testImplementation "junit:junit:$junit_version"
}
```

### ----------------------- Including dependencies from Remote -----------------------------------
mavenCentral --> for maven repository

jcenter --> for Jfrog bintray repository

```
repositories {
    // Use Maven Central for resolving dependencies.
    mavenCentral()
    // Use Jfrog bintray for resolving dependencies.
    jcenter()
}
```

### --------------------- Including dependencies from Remote using url ---------------------------

```
repositories {
    url "http://jcenter.bintray.com/"
}
```


### -------------------- Including dependencies from private maven repository ----------------------

```
repositories {
    // Maven cache from local system (NOT RECOMMENDED)
    mavenLocal()
}

repositories {
    // Private maven repository
    maven {
        url "http://repo.mycompany.com/maven2/"
    }

    // Private Ivy repository
    ivy {
        url "http://repo.mycompany.com/repo/"
    }
}
```

### ------------------------------------- Multiproject build --------------------------------------
The multiproject settings is controlled in *settings.gradle* file in root folder, where we can include
the project folders to build

Each sub project will have their own *build.gradle* file

build.gradle in root folder

```
// To define configuration for all projects
allProjects {
    plugins {
        id "java"
    }
}

// To define configuration at project level in main build.gradle file
project (':app') {
    dependencies project(':app')
}
```

### ---------------------------------------- To install gradle wrapper -----------------------------
gradle wrapper

### ----------------------------------- To install gradle wrapper specific version ------------------
gradle wrapper --gradle-version 7.0

