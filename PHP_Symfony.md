## Installation

Download Installer

` curl -sS https://get.symfony.com/cli/installer | bash`
 
 Make `Symfony` Accessible anywhere
 
 `mv /Users/weaverryan/.symfony/bin/symfony /usr/local/bin/symfony`
 
 ## Create Project
 
 New project
 
 `symfony new cauldron_overflow`
 
 Run local web server
 
 `symfony serve`
 
 ## Routing
 
 Install Annotations Support
 
 `composer require annotations`
 
 Routing can be customized using annotation `@Route`.
 
 | Param           | defines               |
 | --------------- | --------------------- |
 | `name`          | internal name         |
 | `method`        | only match method     |
 | `defaults`      | add default param     |
 | --------------- | --------------------- |
 | `{slug}`        | wildcard route        |
 | `{slug<REGEX>}` | allow match word only | 
 
 Service object can be added in the function parameter.

 
 ```class MyController
{
    /**
     * @Route("/secondPage/{slug}", name="app_homepage", methods="POST")
     */
    public function show($slug, LoggerInterface $logger, ...)
    {
      return new Response(sprintf(
            'Hello "%s"!',
            ucwords(str_replace('-', ' ', $slug))
        ));
    }
}
```

You can see what function you can have

`php bin/console`

Check what route you have

`php bin/console debug:router`

## Recipe - Secure Checker

Scan all dependencies and tell if there exist known security vulnerabilities

`composer require sec-checker`

Run security check 

`php bin/console security:check`

## Recipe - Twig

Writing template

`composer require template`

`config/packages/twig.yaml` defines template file location

To render a template, the class should `extends AbstractController`

Then the function can 
```
public function show($slug)
{
 return $this->render('secondPage/show.html.twig', [
            // pass variable
            'info' => ucwords(str_replace('-', ' ', $slug)),
            'contents' => ['one', 'two', ... ]
        ]);
        

}
```

In `secondPage/show.html.twig`, 

```
<h1>{{ info }}</h1> // <- print info variable
<h2>{{ 'info' }}</h2> // <- print string `info`
{# This is just a comment #}

{% for c in contents %}
<div>{{ c }}</div>

{% endfor %}
// sth else
...

```

[Twig documents in here](https://twig.symfony.com/doc/)

### Template Inheritence

At the top of the file, type  `{% extends 'base.html.twig' %}`

Any HTML inside `block` will be a template and can be reuse
`{% block foo %} ... {% endblock %}`

In `base.html.twig`, call 
```
{% block foo %}
Here define defualt value when block is not defined.
If defined, this will be overrided. Or you may use {{ parent() }} to insert the default value.
{% endblock %}
```

## Recipe for debugging

### Profiler

A helper for debugging
`composer require profiler --dev`
`--dev` means only need when you are developing, won't be used on production

when you want to debug a variable, instead of `var_dump()`, use `dump()`

### Debug pack

`composer require debug`

#### What it do?
- show all the logs for the request in `var/log/dev.log`
- options to show dump in console `php bin/console server:dump

## Assets

`composer require symfony/asset`

Assert the src
`<link rel="stylesheet" href="{{ asset('css/app.css') }}">`

`asset()` does two things 
- deploy in subdirectory: prefix all the path
- Deploy to a CDN: Prefix every path with the URL


## Generate URLs

Direct to homepage
```
<a href="{{ path('app_homepage') }}"> ... </a>
```

If the route has a wildcard, input the second parameter
```
<a href="{{ path('app_show', { slug: 'info' }) }}"> ... </a>
```

## JSON API

For returning JSON response, use `new JSONResponse(['head' => 'tail']);`

## Webpack Encore

#### Prerequisite
Installed node and yarn.

#### Install

Install bundle `wepack-encore-bundle` which helps Symfony to integrate with Webpack Encode.

`composer require encore`

Then install required node library

`yarn install`

To execute Encore, run

`yarn watch`

#### Installing outside library (jQuery)

`yarn add jquery`

in `index.js`, uncomment `import $ from 'jquery';`.



# Docs

### Bundle

- config/bundles.php
- Symfony plugin
- Install a bundle when you add new feature to your app

### Console commands

Show usable services
`php bin/console debug:autowiring twig`
