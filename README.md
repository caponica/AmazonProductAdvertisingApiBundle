Amazon Product Advertising API Bundle
=================

Amazon PAA integration via a Symfony service wrapper around caponica/amazon-paa.

Installation
------------

Install using [composer](http://getcomposer.org) by adding the following in the `require` section of your `composer.json` file:

``` json
    "require": {
        ...
        "caponica/amazon-paa-bundle": "dev-master"
    },
```

Register the bundle in your kernel:

``` php
<?php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = array(
        // ...
        new Caponica\AmazonPaaBundle\CaponicaAmazonPaaBundle(),
    );
}
```


Configuration
-------------

The bundle does not add a service to your project by default. To add the service,
you will need to define the parameters and then the service itself.

To access multiple marketplaces, create multiple services (one for each marketplace).
The simplest way to do this is with parameters (as shown below), but you can use any
method you like to load the relevant configuration parameters and pass them to setConfig.

``` yaml
# app/config/parameters.yml
caponica_amazon_paa_config_de:
    access_key:     your_access_key_de
    secret_key:     your_secret_key_de
    associate_tag:  your_associate_tag_de
    domain_suffix:  'de'
caponica_amazon_paa_config_uk:
    access_key:     your_access_key_uk
    secret_key:     your_secret_key_uk
    associate_tag:  your_associate_tag_uk
    domain_suffix:  'co.uk'
```

``` yaml
# services.yml
    caponica_paa_client_de:
        class:      %caponica_amazon_paa.client.class%
        factory:    [ '%caponica_amazon_paa.client.class%', 'buildFromArray' ]
        arguments:  [ '%caponica_amazon_paa_config_de%' ]
    caponica_paa_client_uk:
        class:      %caponica_amazon_paa.client.class%
        factory:    [ '%caponica_amazon_paa.client.class%', 'buildFromArray' ]
        arguments:  [ '%caponica_amazon_paa_config_uk%' ]
```


Usage
-----

To access the service, just reference it by the service name you defined above. E.g., from a controller:

    /** @var CaponicaAmazonPaa\Client\ApaaClient $apaaClientUk */
    $apaaClientUk = $this->container->get('caponica_paa_client_uk');
    $itemLookupResponse = $apaaClientUk->callItemLookupFull('B009O3VLPW');
