# The Pylon & Rebar Project

PylonMC has two repositries: one contains `rebar` and one contains `pylon`. This allows you to run base using your very own home-baked version of core, which allows you to test new features much more easily. This is what the 'How to get started' section used. We recommend you make changes to both Base and Core using the master repository, and the rest of this guide will assume you're using it.

## Project structure
The project is structured as such:
```
pylon/
  src/
rebar/
  dokka-plugin/
  nms/
  rebar/
  test/
```

`pylon` is the base addon for Rebar. `rebar` has four subprojects: `dokka-plugin`, `nms`, `rebar`, and `test`. `dokka-plugin` contains a custom Dokka plugin we use to help with formatting Javadocs. `nms` contains all the Rebar code that touches server internals. It is in a separate subproject to effectively isolate potentially unstable code from the rest of the project. `rebar` contains the main Rebar code, and `test` contains the integration tests for Rebar.

## Tasks
The Pylon master project contains a few tasks that are useful for development:

| Task                         | Alias               | Description                                                                                                |
|------------------------------|---------------------|------------------------------------------------------------------------------------------------------------|
| `runServer`                  | `runSnapshotServer` | Runs a Minecraft server with the current version of Rebar and Rebar                                        |
| `:pylon-base:runServer`      | `runStableServer`   | Runs a Minecraft server with the latest stable version of Rebar and the current version of Pylon      |
| `:rebar:test:runServer` | `runLiveTests`      | Executes the integration tests for Rebar                                                                   |

!!! danger
    If you are launching the tasks using the aliases from IntelliJ and attach the debugger, you will notice that it will not work. I have no idea why this happens. In order to successfully attach the debugger, you need to run the actual tasks, not the aliases. For example, instead of running `runSnapshotServer`, run `runServer`.