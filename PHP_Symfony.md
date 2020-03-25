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
 
 Routing can be customized using annotation `@Route`
 
 ```class MyController
{
    /**
     * @Route("/secondPage/{slug}")
     */
    public function show($slug)
    {
      return new Response(sprintf(
            'Hello "%s"!',
            ucwords(str_replace('-', ' ', $slug))
        ));
    }
}
```

Check what route you have

