Theme Bundle
============

This bundle provides you the possibility to add themes to each bundle. In your
bundle directory it will look under Resources/themes/<themename> or fall back
to the normal Resources/views if no matching file was found.

[![Build Status](https://secure.travis-ci.org/liip/LiipThemeBundle.png)](http://travis-ci.org/liip/LiipThemeBundle)

## Installation

Installation is a quick (I promise!) 3 step process:

1. Download LiipThemeBundle
2. Configure the Autoloader
3. Enable the Bundle

### Step 1: Download LiipThemeBundle

Ultimately, the LiipThemeBundle files should be downloaded to the
`vendor/bundles/Liip/ThemeBundle` directory.

This can be done in several ways, depending on your preference. The first
method is the standard Symfony2 method.

**Using the vendors script**

Add the following lines in your `deps` file:

```
[LiipThemeBundle]
    git=git://github.com/liip/LiipThemeBundle.git
    target=bundles/Liip/ThemeBundle
```

Now, run the vendors script to download the bundle:

``` bash
$ php bin/vendors install
```

**Using submodules**

If you prefer instead to use git submodules, the run the following:

``` bash
$ git submodule add git://github.com/liip/LiipThemeBundle.git vendor/bundles/Liip/ThemeBundle
$ git submodule update --init
```

### Step 2: Configure the Autoloader

Add the `Liip` namespace to your autoloader:

``` php
<?php
// app/autoload.php

$loader->registerNamespaces(array(
    // ...
    'Liip' => __DIR__.'/../vendor/bundles',
));
```

### Step 3: Enable the bundle

Finally, enable the bundle in the kernel:

``` php
<?php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = array(
        // ...
        new Liip\ThemeBundle\LiipThemeBundle(),
    );
}
```

## Configuration

You will have to set your possible themes and the currently active theme. It
is required that the active theme is part of the themes list.

    # app/config/config.yml
        liip_theme:
            themes: ['web', 'tablet', 'mobile']
            active_theme: 'web'

### Optional

If you want to select the active theme based on a cookie you can add

    # app/config/config.yml
        liip_theme:
            theme_cookie: cookieName

It is also possible to automate setting the theme cookie based on the user agent:

    # app/config/config.yml
        liip_theme:
            theme_cookie: cookieName
            autodetect_theme: true

Optionally ``autodetect_theme`` can also be set to a DIC service id that implements
the ``Liip\ThemeBundle\Helper\DeviceDetectionInterface`` interface.

### Theme Cascading Order

The following order is applied when checking for templates, for example "@BundleName/Resources/template.html.twig"
is located at:

1. Override themes directory: app/Resources/themes/BundleName/template.html.twig
2. Override view directory: app/Resources/BundleName/views/template.html.twig
3. Bundle theme directory: src/BundleName/Resources/themes/template.html.twig
4. Bundle view directory: src/BundleName/Resources/views/template.html.twig

### Change Active Theme

For that matter have a look at the ThemeRequestListener.

If you are early in the request cycle and no template has been rendered you
can still change the theme without problems. For this the theme service
exists at:

    $activeTheme = $container->get('liip_theme.active_theme');
    echo $activeTheme->getName();
    $activeTheme->setName("mobile");

## Contribution

Active contribution and patches are very welcome. To keep things in shape we
have quite a bunch of unit tests. If you're submitting pull requests please
make sure that they are still passing and if you add functionality please
take a look at the coverage as well it should be pretty high :)

First initial vendors:

    php vendor/vendors.php

This will give you proper results:

    phpunit --coverage-text
