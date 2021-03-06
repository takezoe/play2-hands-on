---
title: Define Routings
---

## Setup Bootstrap

A project generated by `sbt new` has `app/views/main.scala.html` as the default layout template. First, add CSS and JavaScript to this file to enable Bootstrap in your project.

```html
@(title: String)(content: Html)

<!DOCTYPE html>
<html lang="en">
  <head>
    <title>@title</title>
    <link rel="stylesheet" media="screen" href="@routes.Assets.versioned("stylesheets/main.css")">
    <link rel="shortcut icon" type="image/png" href="@routes.Assets.versioned("images/favicon.png")">
    @* add from here *@
    <link rel="stylesheet" media="screen" href="//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap-theme.min.css">
    <link rel="stylesheet" media="screen" href="//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css">
    <script src="//netdna.bootstrapcdn.com/bootstrap/3.1.1/js/bootstrap.min.js" type="text/javascript"></script>
    @* add until here *@
  </head>
  <body>
    @content
    <script src="@routes.Assets.versioned("javascripts/main.js")" type="text/javascript"></script>
  </body>
</html>
```

In default, since Play responds `default-src 'self'` as `Content-Security-Policy` header browser can't read CSS or javaScript from external CDN. So you need to add following configuration to `conf/application.conf` to disable `Content-Security-Policy` header.

```
play.filters.headers.contentSecurityPolicy=null
```

## Create Controller

It's time to begin implementing your first Play controller.

Create `UserController` class in `controllers` package with following contents:

```scala
package controllers

import play.api.mvc._
import play.api.data._
import play.api.data.Forms._
import javax.inject.Inject
import scalikejdbc._
import models._

class UserController @Inject()(components: MessagesControllerComponents)
  extends MessagesAbstractController(components) {

  /**
   * List all users
   */
  def list = TODO

  /**
   * Registration (and editing) form
   */
  def edit(id: Option[Long]) = TODO

  /**
   * Register user data
   */
  def create = TODO

  /**
   * Update user data
   */
  def update = TODO

  /**
   * Delete user data
   */
  def remove(id: Long) = TODO

}
```

You may wonder the constructor has `@Inject` annotation. This is for Dependency Injection by Google Guice introduced in Play 2.4.

This controller take following component by DI:

- `MessagesControllerComponents` ... Required for Play's I18N functionality. Actually, this application doesn't use I18N, but it's required for helpers used in HTML templates described later.

Also controller needs to extend `MessagesAbstractController` class to access to database or use I18N functionality.

> **POINT**
>
> * `@Inject` is an annotation for Dependency Injection
> * Controller is need to be injected `MessagesControllerComponents` by DI, and extend `MessagesAbstractController` class
> * `TODO` methods responds `501 NOT_IMPLEMENTED` with a message: `Action not implemented yet.`

## Define Routings

Requests from clients are routed to corresponding controller method according to the routing definition in `conf/routes`.

Add following routings to `conf/routes`:

```bash
# Mapping to /user/list
GET     /user/list                  controllers.UserController.list
# Mapping to /user/edit or /user/edit?id=<number>
GET     /user/edit                  controllers.UserController.edit(id: Option[Long] ?= None)
# Mapping to /user/create
POST    /user/create                controllers.UserController.create
# Mapping to /user/update
POST    /user/update                controllers.UserController.update
# Mapping to /user/remove/<number>
POST    /user/remove/:id            controllers.UserController.remove(id: Long)
```

> **POINT**
>
> * If you don't specify the type of parameter, it's handled as `String`.
