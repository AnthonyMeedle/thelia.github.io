---
layout: home
title: Routing - Modules
sidebar: plugin
lang: en
subnav: plugin_routing
---

<div class="page-header">
    <h1>Modules : <small>Routing</small></h1>
</div>

<div class="alert alert-info">You can use symfony a:b:c notation since v2.1.0-alpha2</div>

You can define your own routing rules for your Module.
Routing definition uses XML files

## How to declare my own routes

All you have to do is to create a file named routing.xml in your Config directory.

## routes syntax

The routing.xml file contains something like this :

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<routes xmlns="http://symfony.com/schema/routing"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://symfony.com/schema/routing http://symfony.com/schema/routing/routing-1.0.xsd">

    <route id="MyModule.home" path="/mymodule/" >
        <default key="_controller">Thelia\Controller\Front\DefaultController::noAction</default>
        <default key="_view">index</default>
    </route>
    
    <route id="MyModule.foo" path="/mymodule/foo" >
        <default key="_controller">Thelia:Front\Default:no</default>
        <default key="_view">foo</default>
    </route>

    <route id="MyModule.create.something" path="/mymodule/something/create">
        <default key="_controller">MyModule\Controller\SomethingController::createAction</default>
    </route>
    
    <route id="MyModule.update.something" path="/mymodule/something/update">
        <default key="_controller">MyModule:Something:update</default> 
        <!-- It is the same as:
        <default key="_controller">MyModule\Controller\SomethingController::updateAction</default> 
        -->
    </route>

</routes>
```

## How to create a controller

A controller is basically a class. This class must extends a class, this class can differ according to its context.

If you are in FrontOffice context you must extend the [`Thelia\Controller\Front\BaseFrontController`](/api/master/Thelia/Controller/Front/BaseFrontController.html) class. On the contrary in Admin context
you must extend the [`Thelia\Controller\Admin\BaseAdminController`](/api/master/Thelia/Controller/Admin/BaseAdminController.html)

Example :

```php
<?php
namespace MyModule\Controller;

use Thelia\Controller\Front\BaseFrontController;

class Something extends BaseFrontController
{
    public function createAction()
    {
        //my code here, you can access to many helpers here, see the api
    }
}
```

If you want to generate URL from route id in controllers, keep attention to use the right router. For example, if you want to use the `generateRedirectFromRoute` with a route defined in your module, you should set the current router properly : 

```php
<?php

class Something extends BaseFrontController
{
    protected $currentRouter = "router.mymodule";
    
    public function redirectAction()
    {
        return $this->generateRedirectFromRoute('mymodule.create.success');
    }
}
```