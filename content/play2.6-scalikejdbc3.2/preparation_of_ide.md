---
title: Setup IDE
---

## Install IntelliJ Scala plugin

Assuming you already have Java8 and IntelliJ installation. You need to install Scala Plugin to your IntelliJ additionally.

* [IntelliJ IDEA] - [Preferences] - [Plugins] - [Install JetBrains plugin...]
* Select [Scala] and click [Download and Install]

IntelliJ Ultimate Edition includes Play plugin in addition to free Scala plugin. This Play plugin offers some Play dedicated features like creation Play project using wizard, and editors for HTML templates and configuration files.

## Import Project

Since IntelliJ Scala Plugin supports sbt natively, you can import projects created by sbt to IntelliJ easily. Click [Import Project] and choose the root directory of your Play project.

![Import project (1)](../images/play2.6-scalikejdbc3.2/open_project_intellij1.png)

![Import project (2)](../images/play2.6-scalikejdbc3.2/open_project_intellij2.png)

You will see a following dialog when importing a project. At the first time, [Project JDK] may be empty. If so, click [New...] and choose the installation directory of JDK and click [OK].

![Import project (3)](../images/play2.6-scalikejdbc3.2/open_project_intellij3.png)

When you modify `build.sbt` to add or remove libraries, you will see the following message on the top right of the window.

![Refresh project](../images/play2.6-scalikejdbc3.2/re-import_project.png)

If you click [Refresh], project is reimported and classpath is updated. If you click [Enable auto-imported], this become top be proceeded automatically in every build.sbt modification.
