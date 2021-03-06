---
title: Path-based tmpfile in PHP
layout: post
tags: [ 'php' ]
description: When paths are more useful than resources.
---

PHP has the [`tmpfile`][1] function for creating a file handle which will automatically be destroyed when it is closed
or when the script ends. PHP also has the [`tempnam`][2] function which takes care of creating the file and returning
the path, but doesn't automatically destroy the file.

To get the best of both worlds (temp file + auto-destroy), I have found this useful:

{% highlight php %}
<?php

function tmpfilepath() {
    $path = stream_get_meta_data(tmpfile())['uri'];

    register_shutdown_function(
        function () use ($path) {
            unlink($path);
        }
    );

    return $path;
}
{% endhighlight %}


 [1]: http://php.net/manual/en/function.tmpfile.php
 [2]: http://php.net/manual/en/function.tempnam.php
