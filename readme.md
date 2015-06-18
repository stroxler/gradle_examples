# Examples
 - `webDist` is a very simple project. The subprojects do not depend on one
   another, but the build settings are shared and the explodedDist task puts
   the war files in the same directory. Run `gradle explodedDist` in the parent
   to see the full example.
 - `smallDependencyExample` is a smallish project where everything depends on
   `shared` (which is really just a silly toy, but that is not the point), and
   also the PersonService depends on the api.
     - The example is so small that you cannot actually run it (there is no cli
       or web interface), but you can tell the dependencies resolved because
       tests pass.
     - The test output is non-verbose, so it is actually hard to tell tests are
       running. But if you have any doubts, try adding an `assertTrue(false)`.
       You can see from this that if gradle test is quiet, that means tests
       passed.
     - Take a look at the compiled project. Gradle winds up automatically
       creating a jar for the `api` and `shared` subprojects even though there
       is no jar task, but it does not create a jar for the `services`. So you
       can see that gradle will automatically make jars for subprojects which
       are dependencies, allowing those jars to be added to the classpath of
       the subprojects depending on them.
 - `smallDependencyExampleMoreCustomized` is very similar; the java code is
   identical. It uses a slightly different configuration in `build.gradle.`
 - `smallDependencyExampleJarWithDeps`: I further modified the
   `smallDependencyExample` by adding a `fatJar` task to the `services`
   project. If you run `gradle fatJar`, you will see that all the dependencies
   were included: run
     `jar tf services/personService/build/libs/personService-all-1.0.jar`
   and you can see that the classes from the parent jars appear here.

TODO: The `fatJar` solution is one way of getting a jar with dependencies, but
maven does it differently: it actually includes the dependencies as jars inside
of the jar. I am still unsure how to do this using gradle. I am also not sure whether
third-party libraries would show up using this version of fatJar; need to play with
it more.

In general, we build all the subprojects together. If you want to build
just one, you would add the `-a` flag to the `gradle` command and either
build from that subdirectory, or specify the project in the gradle command.
