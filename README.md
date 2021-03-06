The Mojo Executor provides a way to to execute other Mojos (plugins) within a Maven plugin, allowing you to easily create Maven plugins that are composed of other plugins.

Downloads
=========

You can download the jars, source, and javadocs from Maven central:

http://repo2.maven.org/maven2/org/twdata/maven/mojo-executor/

Example Usage
=============

This is how you would execute the "copy-dependencies" goal of the Maven Dependency Plugin programmatically:

``` java
executeMojo(
    plugin(
        groupId("org.apache.maven.plugins"),
        artifactId("maven-dependency-plugin"),
        version("2.0")
    ),
    goal("copy-dependencies"),
    configuration(
        element(name("outputDirectory"), "${project.build.directory}/foo")
    ),
    executionEnvironment(
        project,
        session,
        pluginManager
    )
);
```

The project, session, and pluginManager variables should be injected via the normal Mojo injection:

``` java
/**
 * The project currently being build.
 *
 * @parameter expression="${project}"
 * @required
 * @readonly
 */
private MavenProject mavenProject;

/**
 * The current Maven session.
 *
 * @parameter expression="${session}"
 * @required
 * @readonly
 */
private MavenSession mavenSession;

/**
 * The Maven BuildPluginManager component.
 *
 * @component
 * @required
 */
private BuildPluginManager pluginManager;
```

You might need to add other annotations to your Mojo class, depending on the needs of your plugin. Annotations declared by Mojos that you execute are _not_ automatically inherited by your enclosing Mojo.

For example, if you are using the `maven-dependency-plugin`, as in this example, you will need to add `@requiresDependencyResolution <scope>` to your class annotations to ensure that Maven resolves the project dependencies before invoking your plugin.

See the [Mojo API Specification][mojo-api] for details on available annotations. Look at the included [example plugin](mojo-executor-maven-plugin/) for an example of use.

Maven Dependency
================

Add this to your pom.xml:

``` xml
<dependencies>
    <dependency>
        <groupId>org.twdata.maven</groupId>
        <artifactId>mojo-executor</artifactId>
        <version>2.0</version>
    </dependency>
</dependencies>
```

There are a few versions available, and the best one to use will depend on the version(s) of Maven you need to support:

  - 1.0.1 &mdash; Supports Maven 2.x only
  - 1.5   &mdash; Supports both Maven 2.x and Maven 3.x
  - 2.0   &mdash; Supports Maven 3.x only

License
=======

Copyright 2008-2013 Don Brown

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

Contributors
============

Mojo Executor was originally created by [Don Brown][mrdon] (mrdon@twdata.org).

It is currently maintained by [Tim Moore][TimMoore] (tmoore@incrementalism.net).

Thanks to the following contributors, who have provided patches and other assistance:

-   [Matthew McCullough][matthewmccullough]
-   Gili Tzabari (cowwoc@bbs.darktech.org) &mdash; Maven 3 support
-   [Joseph Walton][josephw] &mdash; support for both Maven 2 and Maven 3 in the same artifact

[mrdon]: https://github.com/mrdon
[TimMoore]: https://github.com/TimMoore/
[matthewmccullough]: https://github.com/matthewmccullough
[josephw]: https://github.com/josephw

[mojo-api]: http://maven.apache.org/developers/mojo-api-specification.html
