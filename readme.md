# Kirby log plugin

A little log utility you can use with Kirby 3.
It's a wrapper around [KLogger](https://github.com/katzgrau/KLogger).

⚠️ This plugin is currently a playground for me to test the new Kirby plugin system. Do not use in production _yet_. ⚠️ 

## Installation

Put the `kirby-log` folder in your `site/plugins` folder.
Run `composer install` from this directory.

## Usage

### Default

```

kirbylog()->log("This text will be added to the default log");

```

- Output: `[2018-08-06 17:26:50.376956] [info] This text will be added to the default log`.
- Logfile: `/site/kirbylogs/kirbylog.log`

💡 The logfile will be created automatically when not existant.

## Extended arguments

### 1. Custom log name

```

$kirbyLogger = kirbyLog("my-own-logfile.log");
$kirbyLogger->log("This event will be added to my custom named logfile");

```

- Output: `[2018-08-06 17:26:50.376956] [info] This event will be added to my custom named logfile`.
- Logfile: `/site/kirbylogs/my-own-logfile.log`

### 2. Extended options

Several extended options are available:
- dateFormat: [use PHP syntax](http://php.net/manual/en/function.date.php)
- logFormat: [formatting options](https://github.com/katzgrau/KLogger#log-formatting)
- [appendContext](#3-appendcontext): Enable/Disable

Pass them as follows to the logger using an associative array:

```

$options = array (
    'dateFormat'     => 'Y-m-d G:i:s.u',
    'logFormat'      => false,
    'appendContext'  => true,
);
kirbyLog("infolog.log", $options)->log("Info about something", "info");

```

More info on [KLogger docs](https://github.com/katzgrau/KLogger#additional-options).

### 3. AppendContext

AppendContext can be interesting to include variables to your log.

```

$arr = ["foo", "bar", "baz"];
kirbyLog()->log("My message", "debug", $arr);

```

- Output: 
```
[2018-08-06 17:26:50.373625] [debug] My message
    0: 'foo'
    1: 'bar'
    2: 'baz'
```
- Logfile: `/site/kirbylogs/kirbylog.log`

### Loglevel

As defined by [PSR-3](https://www.php-fig.org/psr/psr-3/#5-psrlogloglevel), you can pass the wanted loglevel as the second argument in the `->log()` method:

```

kirbyLog("kirbylog.log")->log("error", "info");

```

- Output: `[2018-08-06 17:26:50.372955] [error] test`
- Logfile: `/site/kirbylogs/kirbylog.log`

💡 By default the loglevel is `info`. [This can be set in the options](#kirby-configurable-options).

## Kirby configurable options

1. The default location where logfiles will be saved is `/site/kirbylogs/`. You can change `kirbylogs` foldername by using setting it via the options `$kirby->option("bvdputte.kirbylog.logfolder", "myownfoldername");`.
2. The default logname is `kirbylog.log`. Change it with `$kirby->option("bvdputte.kirbylog.logname", "custom-logname.log");`.
3. The default loglevel is `info`. Change it with `$kirby->option("bvdputte.kirbylog.defaultloglevel, "debug");`. Be sure to [use a valid PSR-3 loglevel](#loglevel).
