# dh_usj_maven

The current preferred way to package Java libraries in Debian is to install them not only to `/usr/share/java`, but to the local Maven repo (`/usr/share/maven-repo`) as well. See [Debian Java Packaging](https://wiki.debian.org/Java/Packaging) for more info.

At the same time there is quite a few old official packages that install `.jar`-s to `/usr/share/java` only. One of solutions for this issue is to create additional package that creates necessary directory structure in the local Maven repo, put `.pom` stubs there and creates necessary `.jar` symlinks to the actual `.jar`-s in `/usr/share/java`. `dh_usj_maven` is quick and dirty solution for that.

`dh_usj_maven` works as a debhelper. It expects that it's explicitly called somewhere in `override_dh_auto_install` (don't call it too early!) and looks for `usj_maven.yml` file in `debian` dir of your package.

## usj_maven.yml

```yaml
package: <name of your package>

artifacts:
  - group_id: <group_id>
    artifact_id: <artifact_id>
    version: <version> # default is 'debian'
    jar: <jar_file_name> # the name of the .jar file placed in /usr/share/java;
                         # if omitted, then <artifact_id>.jar is used

  - group_id: <group_id>
    artifact_id: <artifact_id2>

  ...
```