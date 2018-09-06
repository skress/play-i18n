<h1 align="center">Multi-format Messaging module<br/> for Play framework</h1>

**Note: This version supports Play framework 2.6.x. For compatiblity with previous versions see previous releases.**

[Play framework 2](http://playframework.com/) is delivered with default Messaging module using property
files. The syntax is not much convenient as involves a lot of repetition, thus this module delivers
an alternative to supporting YAML format for messages including all features of the language.

[![Build Status](https://travis-ci.org/KarelCemus/play-i18n.svg?branch=master)](https://travis-ci.org/KarelCemus/play-i18n)

## How to add the module into the project

To your SBT `build.sbt` add the following lines:

```scala
libraryDependencies ++= Seq(
  // YAML localization module
  "com.github.karelcemus" %% "play-i18n" % "1.1.1"
)
```

And add the following line into your configuration to disable the original Play's implementation:

```hocon
play.modules.disabled += "play.api.i18n.I18nModule"
```

## How to use the this localization module

The end-user API remains same. The module is smoothly connected to the current `I18n` which delivers `MessagesApi`
as a single access point.

**Example:**
```scala
messages: MessagesApi
messages( "my.simple.key", "With", 3, "parameters" )
```

The module supports multiple message file formats. Right now, it supports the property files
for backward compatibility and YAML files for their convenient syntax. The base file name remains same as is defined
by Play framework: `messages[.lang]` where lang is optional for a particular language mutation. For YAML files the name
pattern is very similar: `messages.yaml[.lang]` again with the optional language. All these files are looked up at the
classpath.

**Example of property file:**
```properties
my.simple.key = The key resolves to {0} {1} {2}
my.simple.value = Some text
my.another.key = Another localization key
```

**Example of YAML file:**
```yaml
my:
  simple:
    key: The key resolves to {0} {1} {2}
    value: Some text
  another.key: = Another localization key
```

The YAML file format brings some beautiful features such as:

- key hierarchy
- multi-line strings
- sub-tree replacement (naming one tree and referencing it elsewhere)

For full list of features see [Wikipedia](http://en.wikipedia.org/wiki/YAML#Examples)

**Note:**
Although YAML supports data types, the module ignores them to preserve the compatibility with the MessagingAPI. 

**Support for ICU message format**

You can switch between the default JDK MessageFormat implementation and the implementation provided by the [ICU Project](http://icu-project.org).

To enable ICU's implementation just set the configuration option `play.i18n.useICUMessageFormat` to `true`.

See `MultiFormatMessagingPluginSpec` for an example how to use the advanced formatting options of the ICU project.

## Configuration

There is already default configuration but it can be overwritten in your `conf/application.conf` file.

| Key                           | Type   | Default                       | Description                                |
|-------------------------------|-------:|------------------------------:|--------------------------------------------|
| play.i18n.path                | String | empty                         | Prefix of messages files                   |
| play.i18n.formats.properties  | Boolean| true                          | Enables the format                         |
| play.i18n.formats.yaml        | Boolean| true                          | Enables the format                         |
| play.i18n.useICUMessageFormat | Boolean| false                         | Enables the support for ICU message format |


## Worth knowing to avoid surprises

The library configuration automatically **disables default Messaging module** and **enables custom implementation**.
You do not have to take care of it but it is good to be aware of it, because it **replaces** the implementation.
