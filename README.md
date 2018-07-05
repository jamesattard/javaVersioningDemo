# VersioningDemo

A quick demo to display how to handle versioning with maven

In order to get this setup you clone this repository into your own github account. Then you follow the description [here](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/) to set your ssh keys up for github.

When this is done you should be able to push new changes using ssh and that's required to push the new release.

### Commands for building

To create a package of your build

```
mvn package
```

To prepare a build and create a new release tag on github

```
mvn release:prepare
```

To perform the actual release and get the source, javadoc and jar-with-dependencies

```
mvn release:perform
```
