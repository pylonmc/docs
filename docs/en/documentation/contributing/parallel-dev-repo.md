# The Parallel Dev Repo Project

PylonMC has a repository called `parallel-dev-repo`. This allows you to run Pylon using your very own home-baked version of Rebar, allowing you to test new features much more easily. We recommend you make changes to both Pylon and Rebar using the repository.

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

The `pylon` directory, unsurprisingly, contains the source code for Pylon.

The `rebar` directory is more complicated. Inside it are four subprojects: `dokka-plugin`, `nms`, `rebar`, and `test`. 

- `dokka-plugin` contains a custom Dokka plugin we use to help with formatting Rebar's Javadocs
- `nms` contains all the Rebar code that touches server internals. It is in a separate subproject to effectively isolate potentially unstable code from the rest of the project
- `rebar` contains the code for Rebar itself
- `test` contains the automated tests for Rebar

## Tasks
The parallel dev repo contains a few tasks that are useful for development:

| Task                         | Alias               | Description                                                                                                |
|------------------------------|---------------------|------------------------------------------------------------------------------------------------------------|
| `runServer`                  | `runSnapshotServer` | Runs a Minecraft server with the current version of Rebar and Pylon                                        |
| `:pylon:runServer`      | `runStableServer`   | Runs a Minecraft server with the latest stable version of Rebar and the current version of Pylon                |
| `:rebar:test:runServer` | `runLiveTests`      | Executes the automated tests for Rebar                                                                        |

