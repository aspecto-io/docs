---
description: Instrument your NestJS application using Aspecto
---

# NestJS

## Auto Instrument

Nest applications are commonly started in one of 2 ways:

* Nest cli (`nest start`)
* Directly with the `node` executable (for example: `node dist/main`)

It is recommended to use ["Auto Instrument" technique](./#auto-instrument) with node's "preload" command line option, to ensure instrumentation is initialized before Nest framework.

### Using Nest Cli

To instrument your Nest application when started with Nest cli, add the ["-e" ("--exec")](https://docs.nestjs.com/cli/usages#nest-start) option to `nest start` like this:

```
nest start -e 'node -r @aspecto/opentelemetry/auto-instrument'
```

This should be added everywhere `nest start` is invoked (`package.json`, scripts, terminal, etc).

### Using Node Executable

To instrument you Nest application when started with node executable, add the ["-r" ("--require")](https://nodejs.org/api/cli.html#-r---require-module) to `node` execution like this:

```
node -r '@aspecto/opentelemetry/auto-instrument' dist/main
```

## Configuration

When instrumenting an application with "Auto Instrument", configuration is supplied via environment variables or `aspecto.json` file.&#x20;

{% hint style="info" %}
`ASPECTO_AUTH` **must be provided.** [Other configuration options](customize-defaults/advanced.md) are optional.
{% endhint %}

### .env File

This part is relevant if you are using `@nestjs/config` package to load environment variables from `.env` file (or if you are loading the file yourself).

If you store Aspecto's SDK config options in `.env` file, it needs to be preloaded before `@aspecto/opentelemetry`, e.g.

```
nest start -e 'node -r dotenv/config -r @aspecto/opentelemetry/auto-instrument'
// or
node -r dotenv/config -r '@aspecto/opentelemetry/auto-instrument' dist/main
```
