---
id: module
title: OgmaModule
layout: ../../../layouts/MainLayout.astro
---

:::info

For all of the `@ogma/nestjs-module` parts, make sure that the `@ogma/nestjs-module` package is installed using whatever package manger you are using for your project.

:::

With that out of the way, the package comes with a [Dynamic Nest Module](https://docs.nestjs.com/fundamentals/dynamic-modules) ready for use so you can just configure it and go.

## Root Configuration

First, we have the root configuration for the package, which should be called once, usually in your `AppModule` or whatever else you end up calling the `RootModule`, which will be imported and used with `NestFactory.create`. Like most Nest packages nowadays, there's both synchronous and async configurations that can be made use of.x

### Synchronous Configuration

```ts
OgmaModule.forRoot({
  service: {},
  interceptor: {}
});
```

### Asynchronous Configuration

```ts
OgmaModule.forRootAsync({
  useClass: OgmaConfigService
});
```

or

```ts
OgmaModule.forRootAsync({
  inject: [ConfigService],
  useFactory: (config) => ({
    service: {},
    interceptor: {}
  })
});
```

### forRoot Options

The options that are required for the above methods can be found in the tables below

#### OgmaModuleOptions

| name | type | required | description |
| --- | --- | --- | --- |
| service | OgmaServiceOptions | false | The options that will be passed on to the Ogma instance |
| interceptor | OgmaInterceptorOptions | false | The options that will be passed to the OgmaModule for the interceptor. This is what sets up the parsers so that the interceptor can do the proper parsing of requests |

#### OgmaServiceOptions

This is a simple partial of the options available for the [Ogma instance](/en/logger#ogma-options). For more details, please consult the `@ogma/logger` docs.

#### OgmaInterceptorOptions

There are four optional properties to the `OgmaInterceptorOptions` ,`http`, `gql`, `ws`, and `rpc`, **but** one of the four must be provided if the `interceptor` option is not `false`. All of these options are of the same type, a default `false` value, or a reference to a class that extends `AbstractInterceptorService`. The `OgmaModule` will take care of instantiating this class for you, so you just need to provide a reference. Please view the [`@ogma/platform-express`](/en/nestjs/http/platform-express) docs for a brief example.

## Feature Configuration

Along with root level configuration, the `OgmaModule` also allows for feature level configuration for setting up [`OgmaServices`](/en/nestjs/service) with specified contexts. These contexts will be added to the logs automatically.

```ts
OgmaModule.forFeature(FooService);
```

You can also use `forFeatures` to register multiple contexts at once

```ts
OgmaModule.forFeatures([FooService, BarService, FooBarService]);
```

And lastly, if you wish to make the injected services request scoped you can pass on the options for that like so

```ts
OgmaModule.forFeature(FooService, { addRequestId: true });
```

or for multiple registrations

```ts
OgmaModule.forFeatures([
  {
    context: FooService,
    options: { addRequestId: true }
  },
  BarService
]);
```
