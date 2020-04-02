## Installation

Download Installer

` curl -sS https://get.symfony.com/cli/installer | bash`
 
 Make `Symfony` Accessible anywhere
 
 `mv /Users/{YOUR_USER_NAME}/.symfony/bin/symfony /usr/local/bin/symfony`
 
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


## API Platform

### Setup

Installation

`composer require api`

In `.env` file, config your DB `DATABASE_URL=sql://root:@127.0.0.1:3306/db_name?serverVersion=5.7`

To generate Entity, run

`composer require maker --dev`

When finish, run

`php bin/console make:entity`

Generate migration

`php bin/console make:migration`

If the database NOT exist, run

`php bin/console doctrine:database:create`

Interactive API is then available in `localhost:3000/api/`

### Collection & Item Operations

#### Collection operation

URLs doesn't include `{id}`

#### Item operation

URL includes `{id}` for getting single item

#### Customization

Above the item class, you may define what operations can be done.

`shortName` define what name you would like the api URL be

```
**
 * @ApiResource(
 *     collectionOperations={"get", "post"},
 *     itemOperations={"get" = {"path"="/getItem/{id}"}, "put", "delete"},
 *     shortName="constants"
 * )
 ```
 
 #### Serializer
 
 > Turn objects into a specific format (XML, JSON, YAML, ...) and the other way around
 
##### Control what to parse

NormalizationContext control what to read, 
DenormalizationContext controls what to write.

```
/**
 * @ApiResource(
 *     normalizationContext={"groups"={"listing:read"}},
 *     denormalizationContext={"groups"={"listing:write"}}
 * )
 ```

#### Customize date field

Get date time to somthing like `5 min ago`, `one month ago`.

`composer require nesbot/carbon`

```
public function getCreatedAtAgo(): string
    {
        return Carbon::instance($this->getCreatedAt())->diffForHumans();
    }
```

#### Controlling Items Per Page

```
 * @ApiResource(
 *     attributes={
 *          "pagination_items_per_page"=10
 *     }
 * )
 ```
 
#### Adding a new Format

Run `php bin/console debug:config api_platform` to get current format

- For global setting

In `config/packages/api_platform.yaml`, add

```
api_platform:
    formats:
        jsonld:
            mime_types:
                - application/ld+json
        json:
            mime_types:
                - application/json
        html:
            mime_types:
                - text/html
```

- For class specific setting 

In `YourClass.php`, 

```
 * @ApiResource(
 *     attributes={
 *          "formats"={"jsonld", "json", "html", "jsonhal", "csv"={"text/csv"}}
 *     }
 * )
 ```
 
 #### Validation
 
 ```
 use Symfony\Component\Validator\Constraints as Assert;
 /**
     * @Assert\NotBlank()
     * @Assert\Length(
     *     min=2,
     *     max=50,
     *     maxMessage="Describe your cheese in 50 chars or less"
     * )
     */
    private $title;
```

#### User Entity

`php bin/console make:user`


# Docs

### Bundle

- config/bundles.php
- Symfony plugin
- Install a bundle when you add new feature to your app

### Console commands

Show usable services
`php bin/console debug:autowiring twig`

### JSON-LD

Give a meaning of the data that machine can understand

#### RDF - Resource Description Framework

- set of rules about how we can "describe" the meaning of data

| key | meaning |
| ----| ---- |
| `@id` | Official key for unique identifier |
| `@type` | define what type this API gives |
| `@context` | gives where `@type` deifnes |
