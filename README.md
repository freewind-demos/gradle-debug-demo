Gradle Debug Demo
=================

How to debug gradle itself.

Use gradle `4.8.1` for example.

Download gradle jars and sources
--------------------------------

```
./gradlew prepareGradle
```

It will download the gradle distribution and unzip it, then generate all the required `.class` and source files to `libs`

- all class files merged to: `gradle-4.8.1-all.jar`
- all source files packaged to: `gradle-4.8.1-all-sources.zip`

If you need to use other versions of gradle, you have to modify the version in `build.gradle`

Open in IDEA
------------

Import the project in IDEA:

```
idea .
```

The `libs/gradle-4.8.1-all.jar` is already configured as a compile dependency in `build.gradle`

Attach the source files to classes
----------------------------------

1. Open `project structure`
2. Click on: `Modules` -> `gradle-debug-demo_main` -> `Dependencies`
3. Double click on: `gradle-4.8.1-all.jar` to show a new dialog
4. Click on the first `+` on the bottom left, choose the `libs/gradle-4.8.1-all-sources.zip`, and click on all the later `OK` buttons

Run gradle on command line with debug arguments
-----------------------------------------------

```
cd thisProject
./gradlew compileJava -Dorg.gradle.debug=true --no-daemon
```

Or, another way:

```
export GRADLE_OPTS="-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=5005"
./gradlew compileJava
```

It will hang and wait for remote debug connection.

Define a remote debug task in IDEA
----------------------------------

Back to the project in IDEA:

1. Click on `Edit Configurations` on the top right
2. Add a new `remote` configuration
3. Give the task a name, like `remote gradle`
4. Make sure the port is `5005` (which is default)
5. Click on `OK` button to close the dialog

Set breakpoints
---------------

1. Press `Cmd + N`, and search `BasicScript` (make sure its package is `org.gradle.groovy.scripts`)
2. Press `Enter` to open it, should see the raw Java source (not decompiled one), since we have already attached the sources
3. Find `public Object invokeMethod(String name, Object args)`, and give a breakpoint in the body (click on the left side of the code line, will see a red dot)

Notice this is not the very beginning of gradle running, but is a good start point.

Also you can set a breakpoint on the `DefaultProject.dependencies` method, which corresponds to:

```
dependencies {
}
```

section in `build.gradle`

Debug remote gradle
-------------------

1. Click on the `debug` button to start the `remote gradle` task on the top right

There should be some output in IDEA, and after a while, the debug panel is shown, and it paused at the breakpoint we made.

Let's start debugging ...

(It's quite boring to do the setup, but I couldn't find a better solution. Please tell me if you have one, thanks)

