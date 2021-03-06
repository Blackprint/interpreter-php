<p align="center"><a href="#" target="_blank" rel="noopener noreferrer"><img width="150" src="https://avatars2.githubusercontent.com/u/61224306?s=150&v=4" alt="Blackprint"></a></p>

<h1 align="center">Blackprint Engine for PHP</h1>
<p align="center">Run exported Blackprint on PHP environment.</p>

<p align="center">
    <a href='https://github.com/Blackprint/Blackprint/blob/master/LICENSE'><img src='https://img.shields.io/badge/License-MIT-brightgreen.svg' height='20'></a>
</p>

This repository is designed to be used together with [Blackprint](https://github.com/Blackprint/Blackprint) as the engine on PHP environment.

## Documentation
> Warning: This project haven't reach it stable version (semantic versioning at v1.0.0)<br>

```php
<?php
// Create Blackprint Engine instance, `instance` in this documentation will refer to this
$instance = new Blackprint\Engine();
```

### Register new node interface type
An interface is designed for communicate the node handler with the PHP's runtime API. Because there're no HTML to be controlled, this would be little different with the browser version.

```php
<?php
$instance->registerInterface('logger', function($iface, $bind){
    // `bind` is used for bind `iface` property with a function
    // Because PHP lack of getter and setter, implementation would be little different

    $myLog = '...';
    bind([
        'log'=> function($val=null) use(&$myLog) {
            // Getter
            if($val === null)
                return $myLog;

            // Setter
            $myLog = $val;
            echo $val;
        }
    ]);

    // After that, you can get/set from `iface` like a normal property
    // iface.log === '...';

    // In the iface object, it simillar with: https://github.com/Blackprint/Blackprint
    $iface->clickMe = function(){/*...*/};
});
```

## Node handler registration
This is where we register our logic with Blackprint.<br>
If you already have the browser version, you can just copy it without changes.<br>
It should be compatible if it's not accessing any Browser API.<br>

```php
<?php
$instance->registerNode('myspace/button', function($node, $iface){
    // Use iface handler from instance.registerInterface('button')
    $iface->interface = 'button';
    $iface->title = "My simple button";

    // Called after `.button` have been clicked
    $node->onclicked = function($ev){
        echo "Henlo $ev";
    };
});

$instance->createNode('math/multiply', /* [..options..] */);
```

### Example
![fvQRA2wXt8](https://user-images.githubusercontent.com/11073373/82133948-eca50c80-981b-11ea-9e88-0fafd2841a41.png)

This repository provide an example with the JSON too, and you can try it with PHP CLI:<br>

```sh
# Change your working directory into empty folder first
$ git clone --depth 1 https://github.com/Blackprint/engine-php .
$ composer install
$ php ./example/init.php
```