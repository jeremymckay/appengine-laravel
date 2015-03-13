appengine-laravel  needs a laravel 5 update
===========================================

Short example or setting up laravel to run on Google Appengine.

* Google cheat sheet is here http://bit.ly/1vNrreL  it also contains links to gist for all the files that need modification.

If you use this example and do a composer up you will lose the changes below. 

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

references

Project files came from lynda.com (Joseph Lowery)
http://www.lynda.com/Laravel-tutorials/Up-Running-Laravel/166513-2.html

Google Appengine Req
Download google appengine sdk
https://cloud.google.com/appengine/downloads
Download google cloud sdk
https://cloud.google.com/sdk/

Google Appengine Ref
https://gae-php-tips.appspot.com/2013/10/22/getting-started-with-laravel-on-php-for-app-engine/



