# Background

`com.lambdaisland/classpath` uses `tools.deps.alpha`, which depends on `org.codehaus.plexus.util.xml.Xpp3Dom` which was removed in plexus-utils 4.x

```
bin/launchpad
    → com.lambdaisland/classpath 0.6.58
      → org.clojure/tools.deps.alpha 0.14.1222
        → clojure.tools.deps.alpha.util.maven → Xpp3Dom (removed in plexus-utils 4.x)
```

# The symptom

So running `bin/launchpad --verbose` in any project that has `plexus-utils` 4.x as a dependency, either directly,
or say via `com.lambdaisland/garden → resources-optimizer-maven-plugin` results in the error

```
Execution error (ClassNotFoundException) at java.net.URLClassLoader/findClass
                (URLClassLoader.java:377).
org.codehaus.plexus.util.xml.Xpp3Dom
```

See [output.txt](output.txt) for the full stack trace.

Workaround: add `org.codehaus.plexus/plexus-xml {:mvn/version "4.1.1"}` to deps.edn.

Potential fix: upgrade `com.lambdaisland/classpath` to use `tools.deps` (without the alpha).

Related: https://github.com/lambdaisland/classpath/issues/10
