appengine-laravel
=================

Short example or setting up laravel to run on Google Appengine.

* Google cheat sheet is here http://bit.ly/1vNrreL

I uploaded all the vendor files because one file, defeats the purpose of composer.  But will give you a runable example on download.

/vendor/laravel/framework/src/Illuminate/Foundation/Application.php

Requires these changs to the bindInstallPaths function:

```
public function bindInstallPaths(array $paths)
{
    if (realpath($paths['app'])) {
        $this->instance('path', realpath($paths['app']));
    }
    elseif (file_exists($paths['app'])) {
        $this->instance('path', $paths['app']);
    }
    else {
        $this->instance('path', FALSE);
    }

    foreach (array_except($paths, array('app')) as $key => $value)
    {
        if (realpath($value)) {
            $this->instance("path.{$key}", realpath($value));
        }
        elseif (file_exists($value)) {
            $this->instance("path.{$key}", $value);
        }
        else {
            $this->instance("path.{$key}", FALSE);
        }
    }
}
```

So If you rerun composer up, make sure to make the above changes.  Or the project will not run on appengine
