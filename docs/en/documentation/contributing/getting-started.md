!!! info "This guide covers contributing to both Pylon and Rebar, as the process is very similar."

## What should I contribute?

We generally welcome contributions for both Rebar and Pylon.

We use GitHub issues to keep track of tasks. Good first issues are labelled with the 'good first issue' tag. You can view good first issues for Rebar [here](https://github.com/pylonmc/rebar/issues?q=state%3Aopen%20label%3A%22good%20first%20issue%22) and for Pylon [here](https://github.com/pylonmc/pylon/issues?q=state%3Aopen%20label%3A%22good%20first%20issue%22). Picking one of these issues is a great place to start.

If you'd like to do something which does not have an issue already (you can check this by searching for keywords in issues), we recommend you open an issue and assign yourself to it before completing the task. This helps avoid duplicate work and lets others know what you are doing.

If you are planning on making a major change, we strongly recommend discussing with the team beforehand on the Pylon Discord server.

If you have any questions about contributing, feel free to ask us on the Pylon Discord.

## Prerequisites

### Contributing to Rebar
- A GitHub account
- A Git/GitHub client such as [GitHub Desktop](https://github.com/apps/desktop)
- Knowledge of [Kotlin](https://kotlinlang.org/)
- A Kotlin IDE (such as IntelliJ)

### Contributing to Pylon
- A GitHub account
- A Git/GitHub client such as [GitHub Desktop](https://github.com/apps/desktop)
- Knowledge of Java
- A Java IDE (such as IntelliJ)

!!! question "What is Kotlin?"
    Kotlin is a language similar to Java, but with more modern features and concise syntax. If you know Java, you'll be able to pick up Kotlin very quickly.

## How to get started

We use the **parallel dev repository** to develop Rebar and Pylon in parallel. We recommend you use this repository when contributing to Pylon, Rebar, or both.

1. Clone the `parallel-dev-repo` repository: `git clone https://github.com/pylonmc/parallel-dev-repo` (or use a GUI like Github Desktop)
2. If you're using IntelliJ, it should clone the `rebar` and `pylon` repositories into the project automatically. If not, run `./gradlew`.

See the [parallel dev repo docs page](parallel-dev-repo.md) for more information about the parallel dev repository.

### Contributing to Rebar

3. [Fork Rebar](https://github.com/pylonmc/rebar/fork) on GitHub
4. Delete the `rebar` directory
5. Clone your fork in place of the `rebar` directory you just deleted
6. Make your changes
7. [Test your changes](testing-your-changes.md)
8. Commit and push your changes to your fork
9. Open a pull request

### Contributing to Pylon

3. [Fork Pylon](https://github.com/pylonmc/pylon/fork) on GitHub
4. Delete the `pylon` directory
5. Clone your fork in place of the `pylon` directory you just deleted
6. Make your changes
7. [Test your changes](testing-your-changes.md)
8. Commit and push your changes to your fork
9. Open a pull request

## I'm stuck, what next?

1. If it's Pylon/Rebar specific, check if it's in the docs. If it's not Pylon/Rebar specific, google it.
2. Search issues on the relevant repository to see if it's been mentioned
3. Search relevant terms on our Discord server to see if it's been discussed before
4. Ask a question on our Discord server

