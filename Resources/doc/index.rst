==================
HudsonElRteBundle
==================

Add bundle
----------

First we need add jQuery & jQueryUI: see http://github.com/dbykadorov/HudsonJQueryBundle for details
or add scrips manually in <head> section.

Second step - add elrte bundle as submodule:

::

    git submodule add git://github.com/dbykadorov/HudsonElRteBundle.git src/Hudson/ElRteBundle

Init bundle
-----------

::

    // app/autoload.php

    $loader->registerNamespaces(array(
        // ...
        'Hudson' => __DIR__ . '/../src',
    ));

   // app/AppKernel.php

    public function registerBundles()
    {
        $bundles = array(
            // ...
            new Hudson\ElRteBundle\HudsonElRteBundle(),
        );
    // ...
    }

Don't forget to install assets:

::

    php app/console assets:install

Use bundle
----------

Firs we need to add bundled template which includes ElRte and contains init function:

::

    {% include 'HudsonElRteBundle::_elRte.html.twig' with {'language': 'ru'} %}

Language parameter is optional. Default language - en (used for elrte and elfinder translation).

Then we need to init editor:

::

    <script type="text/javascript" charset="utf-8">
        elRTEInit('target-textarea-id', {height: 100});
    </script>

Function elRTEInit(elementId, opts) accepts two parameters:

:elementId: id of textarea element which will be replaced by ElRte
:opts: a javascript object with native elrte options (http://elrte.org/redmine/projects/elrte/wiki/Docs_EN#Options)

Note that fmOpen option by default initialized for ElFinder (http://elrte.org/elfinder) integration.

Full list of options for elrte-1.3

::

    {
        doctype: '<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">', /* String - DocType of editor window (iframe). Default - <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"> */
        cssClass: '', /* String - CSS class for editor */
        cssfiles: [], /* Array - Array of css files which will be included in editor (iframe) */
        absoluteURLs: false, /* Boolean - Make image URLs absolute */
        allowSource: true, /* Boolean - Allow edit in HTML */
        lang: 'en', /* String - Interface language (requires inclusion of language file), default - English */
        styleWithCSS: true, /* Boolean - If true - text will be formated using span-tag with style attribute, else - semantic tags like strong, em and other */
        height: 200, /* Number - Height of editor window in pixels */
        width: 800, /* Number - Width of editor window in pixels */
        fmAllow: true, /* Boolean - Allow use of file manager */
        fmOpen: function(){}, /* Function(callback) - Function which will be called to open file manager. Argument callback - function which editor passes to file manager on open. File manager must call this function with using URL of selected file as argument. By default configured to use elfinder */
        toolbar: 'defaultToolbar' /* String - Toolbar to use. ElRteBundle define 'defaultToolbar' with basic functions, you also can use elrte toolbars: 'tiny', 'compact', 'normal', 'complete', 'maxi' or define your own toolbar (http://elrte.org/redmine/projects/elrte/wiki/Docs_EN#Custom-toolbar) */
    }

Known issues
------------

- If allowSource = true, HTML5 validation (required attribute) always fails. Use <form novalidate='novalidate'> tag, or disable required option for form field with elrte editor, or set allowSource = false.
