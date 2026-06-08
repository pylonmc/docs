# Custom Dokka

Pylon uses a custom version of Dokka to generate Javadocs. This is because the default Dokka output is very buggy and doesn't look very good. Seggan has submitted pull requests to the Dokka project to fix the issues, but they haven't been merged yet, so in the meantime we use a custom version. If you wish to see the "fixed" doc output, here are the steps:

1. Clone the `pylonmc/dokka` repository: `git clone https://github.com/pylonmc/dokka`
2. Checkout the `pylon` branch: `git checkout pylon`
3. In the root directory, execute `./gradlew publishToMavenLocal -Pversion=2.1.0-pylon-SNAPSHOT`. Note that as Dokka is a very large project, the first build will take a long time. On Seggan's decent-ish laptop, the first run took about 10 minutes. Subsequent builds will be much faster as long s you don't delete the build cache.
4. Now, in the Pylon master project, execute `./gradlew :rebar:rebar:dokkaGenerate -PusePylonDokka=true`. The generated output will be under `pylon/rebar/rebar/build/dokka`.

