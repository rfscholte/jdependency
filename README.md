[![Linux/Mac Build Status](https://secure.travis-ci.org/tcurdt/jdependency.png)](http://travis-ci.org/tcurdt/jdependency)
[![Windows Build Status](https://img.shields.io/appveyor/ci/tcurdt/jdependency/master.svg?label=windows)](https://ci.appveyor.com/project/tcurdt/jdependency/branch/master)
[![Coverage Status](https://coveralls.io/repos/tcurdt/jdependency/badge.svg?branch=master&service=github)](https://coveralls.io/github/tcurdt/jdependency?branch=master)
[![Maven Central](https://img.shields.io/maven-central/v/org.vafer/jdependency.svg?maxAge=86400)](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.vafer%22%20AND%20a%3A%22jdependency%22)
[![Join the chat at https://gitter.im/tcurdt/jdependency](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/tcurdt/jdependency?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)


# jdependency - explore your classpath

jdependency is small library that helps you analyze class level dependencies,
clashes and missing classes.

Check the documentation on how to use it with [javadocs](https://tcurdt.github.com/jdependency/release/2.4.0/apidocs/) and a source
[xref](http://tcurdt.github.com/jdependency/release/2.4.0/xref/) is also available.

## Where to get it

The jars are available on [maven central](https://repo1.maven.org/maven2/org/vafer/jdependency/).
The source releases you can get in the [download section](https://github.com/tcurdt/jdependency/downloads).

If feel adventures or want to help out feel free to get the latest code
[via git](https://github.com/tcurdt/jdependency/tree/master).

    git clone git://github.com/tcurdt/jdependency.git

## How to use it

    final File jar1 = ...
    final File jar2 = ...

or

    final Path jar1 = ...
    final Path jar2 = ...

### finding classpath clashes

    final Clazzpath cp = new Clazzpath();
    cp.addClazzpathUnit(jar1, "jar1.jar");
    cp.addClazzpathUnit(jar2, "jar2.jar");

    final Set<Clazz> clashed = cp.getClashedClazzes();
    for(Clazz clazz : clashed) {
      System.out.println("class " + clazz + " is contained in " + clazz.getClasspathUnits());
    }

### finding missing classes

    final Clazzpath cp = new Clazzpath();
    cp.addClazzpathUnit(jar1, "jar1.jar");

    final Set<Clazz> missing = cp.getMissingClazzes();
    for(Clazz clazz : missing) {
      System.out.println("class " + clazz + " is missing");
    }

### finding unused classes

    final Clazzpath cp = new Clazzpath();
    final ClazzpathUnit artifact = cp.addClazzpathUnit(jar1, "artifact.jar");
    cp.addClazzpathUnit(jar2, "dependency.jar");

    final Set<Clazz> removable = cp.getClazzes();
    removable.removeAll(artifact.getClazzes());
    removable.removeAll(artifact.getTransitiveDependencies());

    for(Clazz clazz : removable) {
      System.out.println("class " + clazz + " is not required");
    }

## Related projects

* [maven-shade-plugin](https://maven.apache.org/plugins/maven-shade-plugin/)
* [jarjar](http://code.google.com/p/jarjar/)
* [proguard](http://proguard.sourceforge.net/)
* [gradle-lean](https://github.com/cuzfrog/gradle-lean)

## License

All code and data is released under the Apache License 2.0.