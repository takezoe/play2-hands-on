---
title: Setup Database
---

## Setup tools

Download [h2.zip](../downloads/h2.zip), then unzip to same directory as `play2-hands-on` project as:

```
+-/play2-hands-on
|   |
|   +-/app
|   |
|   +-/conf
|   |
|   +-...
|
+-/h2
    |
    +-create.sql
    |
    +-data.mv.db
    |
    +-...
```

## Start H2 Database

**Windows**

Double click `h2/start.bat`. Database will be started with below tables from its beginning.

**Mac**

```
cd h2/
sh start.sh
```

Don't close the terminal after starting database.

![Tables](../images/play2.6-scalikejdbc3.2/er_diagram.png)

## Generate Models

We need create model classes to use type-safe API of ScalikeJDBC, but we don't need to write them by hand because ScalikeJDBC offers a sbt plugin for generating model classes from existing database schema.

To enable this sbt plugin in `play2-hands-on` project, add following lines to `project/plugins.sbt`:

```scala
libraryDependencies += "com.h2database" % "h2" % "1.4.196"
addSbtPlugin("org.scalikejdbc" %% "scalikejdbc-mapper-generator" % "3.2.2")
```

Also we need to create `project/scalikejdbc.properties` with following contents:

```properties
# ---
# jdbc settings

jdbc.driver=org.h2.Driver
jdbc.url=jdbc:h2:tcp://localhost/data
jdbc.username=sa
jdbc.password=sa
jdbc.schema=PUBLIC

# ---
# source code generator settings

generator.packageName=models
# generator.lineBreak: LF/CRLF
generator.lineBreak=LF
# generator.template: interpolation/queryDsl
generator.template=queryDsl
# generator.testTemplate: specs2unit/specs2acceptance/ScalaTestFlatSpec
generator.testTemplate=ScalaTestFlatSpec
generator.encoding=UTF-8
# When you're using Scala 2.11 or higher, you can use case classes for 22+ columns tables
generator.caseClassOnly=true
# Set AutoSession for implicit DBSession parameter's default value
generator.defaultAutoSession=true
# Use autoConstruct macro (default: false)
generator.autoConstruct=false
# joda-time (org.joda.time.DateTime) or JSR-310 (java.time.ZonedDateTime java.time.OffsetDateTime)
generator.dateTimeClass=java.time.OffsetDateTime
```

Then add following line to `buils.sbt`. `scalikejdbcGen` task is now available in your project.

```scala
enablePlugins(ScalikejdbcPlugin)
```

Let's generate model classes. Run following command in the root directory of `play2-hands-on` project.

```
sbt "scalikejdbcGenAll"
```

Then model classes generated into `app/models` package of `play2-hands-on` project.

## Database Configuration

Add database configuration to `conf/application.conf` of `play2-hands-on` project. It also contains configuration for integrating Play with ScalikeJDBC.

```properties
# Database configuration
# ~~~~~
# You can declare as many datasources as you want.
# By convention, the default datasource is named `default`
db.default.driver=org.h2.Driver
db.default.url="jdbc:h2:tcp://localhost/data"
db.default.username=sa
db.default.password=sa

# ScalikeJDBC original configuration
#db.default.poolInitialSize=10
#db.default.poolMaxSize=10
#db.default.poolValidationQuery=

scalikejdbc.global.loggingSQLAndTime.enabled=true
scalikejdbc.global.loggingSQLAndTime.singleLineMode=false
scalikejdbc.global.loggingSQLAndTime.logLevel=debug
scalikejdbc.global.loggingSQLAndTime.warningEnabled=true
scalikejdbc.global.loggingSQLAndTime.warningThresholdMillis=5
scalikejdbc.global.loggingSQLAndTime.warningLogLevel=warn

play.modules.enabled += "scalikejdbc.PlayModule"
# scalikejdbc.PlayModule doesn't depend on Play's DBModule
play.modules.disabled += "play.api.db.DBModule"
```
