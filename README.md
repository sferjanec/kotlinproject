# Debugging Kotlin program using vscode


## Summary

This example project shows how to configure VSCode for building, debugging, and running Kotlin projects.

This project can be run on both Windows and Linux. I have not tested with Mac, but should be possible without much change, using brew to install both kotlin and gradle.


## Prerequisites

* VSCode
* Java(OpenJDK) 1.8 or higher

The instructions are skipped to install the above prerequisites.


## Procedures

### 1. Install kotlin and gradle

- This project uses Kotlin 1.3.72
Download from https://github.com/JetBrains/kotlin/releases/tag/v1.3.72
Install kotlin-compiler-1.3.72.zip
Append <install_dir>/kotlinc/bin to Path environment variable
Example: "C/Development/kotlin-compiler-1-3.72kotlinc/bin"
test installation: enter command kotlinc -version. You should see "info: kotlinc-jvm 1.3.72 as output"


- This project uses Gradle 6.7.2
Download from https://gradle.org/install/
Follow install instructions for your platform
test installation: enter command gradle -version. You should see "--------- Gradle 6.7.1 ----------------"


### 2. create a kotlin sample app

```shell
% mkdir kotlinproject
% cd kotlinproject
% gradle init --type=kotlin-application
# Select all default choices is OK
 
# Have a try is everything is ok
% ./gradlew build

BUILD SUCCESSFUL in 3s
8 actionable tasks: 8 executed
```

### 3. Install VSCode extensions

``` 
From within VSCode: CTRL-P: Extensions: Install Extensions
Kotlin
Kotlin Language
Java Extension Pack
Java Test Runner
Debugger for Java


Select "Install All" to the dialog showed, which will install the 2 extensions wrote above.

Open App.kt file, and you will see "syntax highlight", "auto compleletion" working.
```

### 4. Task Settings to Build

```
% vi .vscode/tasks.json or CTRL-P : Tasks: Configure Default Build Task

----- Input the content below -----

{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "./gradlew build -x test",
            "problemMatcher": [],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        },
        {
            "label": "clean",
            "type": "shell",
            "command": "./gradlew clean",
            "problemMatcher": []
        },
        {
            "label": "run",
            "type": "shell",
            "command": "./gradlew run",
            "problemMatcher": []
        },
        {
            "label": "test",
            "type": "shell",
            "command": "./gradlew test",
            "problemMatcher": []

        }
    ]
}

```

In vscode menu,  Terminal -> Run Task... 
You can execute  gradlew's build / run / test tasks.

### 5. Debug settings:

```
% vi .vscode/launch.json or CTRL-P : Debug : open launch.json

----- Input content below -----

{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "kotlin",
            "request": "launch",
            "name": "Kotlin Launch",
            "projectRoot": "${workspaceFolder}/app",
            "mainClass": "com/crispyecho/kotlinproject/AppKt", **
            "preLaunchTask": "build"
        }
    ]
}

```

* Rewrite the mainClass to fit to your project.
** rewrite com/crispyecho/kotlinproject to match your package name. This will be the path to your App.kt file. 
Kotlin generates a main Java class to match the name of your main class. If your main Kotlin class file is App.kt, Kotlin will create a corresponding 
Java main class called AppKt.

Set some breakpoint,  debug the program by vscode's Debug tab  -> Run button. 
