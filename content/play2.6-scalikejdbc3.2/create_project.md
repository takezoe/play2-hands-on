---
title: Create Project
---

## Installing sbt

First of all, you need to install sbt that is a de-facto standard build tool in Scala.

**Windows**

Download an installer from a following link:

https://github.com/sbt/sbt/releases/download/v1.1.4/sbt-1.1.4.msi

**Mac**

Install using Homebrew.

```
brew update
brew install sbt@1
```

After installation, please make sure if sbt command is available and its version is higher than 1.1.4.

```
sbt sbtVersion
[info] Loading project definition from /Users/takako.shimamoto/project
[info] Set current project to takako-shimamoto (in build file:/Users/takako.shimamoto/)
[info] 1.1.4
```

## Create New Project

Run following command in the terminal to create a project:

```
sbt new playframework/play-scala-seed.g8 --branch 2.6.x
```

This command will ask some project information to you. In this training, the project name is `play2-hands-on` and use default value for other items.

![Create a project](../images/play2.6-scalikejdbc3.2/create_project.png)

Then, add configuration to use ScalikeJDBC to `play2-hands-on/build.sbt`.

```scala
name := """play2-hands-on"""
organization := "com.example"

version := "1.0-SNAPSHOT"

lazy val root = (project in file(".")).enablePlugins(PlayScala)

scalaVersion := "2.12.4"

libraryDependencies += guice
libraryDependencies += "org.scalatestplus.play" %% "scalatestplus-play" % "3.1.2" % Test

// **** add from here ****
libraryDependencies ++= Seq(
  "com.h2database" % "h2" % "1.4.196",
  "org.scalikejdbc" %% "scalikejdbc" % "3.2.2",
  "org.scalikejdbc" %% "scalikejdbc-config" % "3.2.2",
  "org.scalikejdbc" %% "scalikejdbc-play-initializer" % "2.6.0-scalikejdbc-3.2"
)
// **** add until here ****

// Adds additional packages into Twirl
//TwirlKeys.templateImports += "com.example.controllers._"

// Adds additional packages into conf/routes
// play.sbt.routes.RoutesKeys.routesImport += "com.example.binders._"
```

## Run Your Application

Move into `play2-hands-on` and run the project by following command:

```
sbt run
```

Access http://localhost:9000/ in your browser, then you will see following message.

![Welcome to Play2](../images/play2.6-scalikejdbc3.2/welcome.png)

> **POINT**
>
> * While running application by `sbt run`, hot deployment is available. So modification of source code is applied to running application immediately
> * You can stop your application by CTRL+D
> * Continuous modification of source code with `sbt  run` often causes sudden process shutdown or freeze by the lack of memory
> * When your application shutdown suddenly, please re-run `sbt run`
> * When your application freezes, close-and-reopen your terminal and run `sbt run` again
