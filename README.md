# maven-repo

Maven repository for ucombinator libraries and needed third party dependencies. To add a library, deploy locally to a checkout of the `gh-pages` branch, commit, and push. The repository is accessible at https://ucombinator.github.io/maven-repo. See the `gh-pages` branch on github to see the published libraries and versions.

## Using from sbt

To depend on a library hosted here from an sbt project, add something like this to your `build.sbt`, replacing the soot dependency with the library you're interested in:

```
resolvers += "Ucombinator maven repository on github" at "https://ucombinator.github.io/maven-repo"

libraryDependencies ++= Seq(
    "org.ucombinator.soot" % "soot-all-in-one" % "nightly.20150205"
)
```

## Deploying 3rd party jars

Maven is the best option for this.

```
mvn deploy:deploy-file \
    -Dfile="</your/path/to/jar.jar>" \
    -DgroupId=<your-groupId>
    -DartifactId=<your-artifactId> \
    -Dversion=<your-version> \
    -Dpackaging=jar \
    -Durl="file://</your/path/to/checkout>" \
    -DrepositoryId=org.ucombinator
```

## Deploying sbt projects

Add something like the following to your `build.sbt`

```
// Publish to the maven repo indicated on the command line by
// `-Ducombinator-repo="/path/to/repo"` else to the folder "deploy" in the current directory
publishTo := Some(Resolver.file("file",  new File(sys.props.getOrElse("ucombinator-repo", default = "deploy"))))


// Publish with maven and make this a pure java project
publishMavenStyle := true
```

then use

```
sbt -Ducombinator-repo="/path/to/checkout" publish
```
