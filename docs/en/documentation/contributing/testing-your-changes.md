## Testing your changes

Please test your changes before opening a pull request.

To test your changes, you should run a local server with your modified versions of Rebar and/or Pylon. If you are using the parallel dev repo, this is easy. In IntelliJ, open the gradle panel on the right, open 'parallel-dev-repo', open 'run paper', and double click 'runServer'. This will run a server with your local versions of Rebar and Pylon on it. Otherwise, you can run `./gradlew runServer` from the command line. You can join the server by starting Minecraft and connecting to `localhost`.

## Debugging

You can use a debugger on your code easily with IntelliJ. Ensure that the 'parallel-dev-repo \[runServer\]' run configuration is selected, and then click the debug button. This will launch a server and attach the debugger to it.

By default, the debugger will also attach to gradle tasks, including compilation, which causes a significant slowdown. To fix this, edit the run configuration, click 'modify options', and uncheck 'debug gradle scripts'.

!!! danger
    If you are launching the tasks using the aliases from IntelliJ and attach the debugger, you will notice that it does not work. I have no idea why this happens. In order to successfully attach the debugger, you need to run the actual tasks, not the aliases. For example, instead of running `runSnapshotServer`, run `runServer`.

## Automated tests

Rebar has a set of automated tests. Due to the limited and annoying nature of automatically testing a Minecraft plugin, they should only be added for critical functionality such as block storage and recipes.

!!! warning
    Do not expect Rebar's automated tests to catch any issues in your code unless you are modifying specific parts of the core systems that they test, such as parts of the fluid ticking logic.

