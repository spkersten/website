---
title: Web renderers
description: How to choose a web renderer for running and building a web app.
---

When running and building apps for the web, you can choose between two different
renderers. This page describes both renderers and how to choose the best one for
your needs. The two renderers are:

**HTML renderer**
: Uses a combination of HTML elements, CSS, Canvas elements, and SVG elements.
  This renderer has a smaller download size.

**CanvasKit renderer**
: This renderer is fully consistent with Flutter mobile and desktop, has
  faster performance with higher widget density, but adds about 2MB in
  download size.

## Command line options

The `--web-renderer` command line option takes one of three values, `auto`,
`html`, or `canvaskit`.

* `auto` (default) - automatically chooses which renderer to use. This option
  chooses the HTML renderer when the app is running in a mobile browser, and
  CanvasKit renderer when the app is running in a desktop browser.
* `html` - always use the HTML renderer
* `canvaskit` - always use the CanvasKit renderer

This flag can be used with the `run` or `build` subcommands. For example:

```
flutter run -d chrome --web-renderer html
flutter build web --web-renderer canvaskit
```

This flag is ignored when a non-browser (mobile or desktop) device
target is selected.

## Runtime configuration

To override the web renderer at runtime:

* Build the app with the `auto` option.
* Prepare a configuration object with the `renderer` property set to
  `"canvaskit"` or `"html"`.
* Pass that object to the `engineInitializer.initializeEngine(configuration);`
  method on your [Flutter Web app initialization][web-app-init].

```javascript
let useHtml = // ...
_flutter.loader.loadEntrypoint({
  onEntrypointLoaded: async function(engineInitializer) {
    // Run-time engine configuration
    let config = {
      renderer: useHtml ? "html" : "canvaskit",
    }
    let appRunner = await engineInitializer.initializeEngine(config);

    await appRunner.runApp();
  }
});
```

The web renderer can't be changed after the Flutter engine startup process
begins in `main.dart.js`.

{{site.alert.note}}
  As of **Flutter 3.7.0**,  setting a `window.flutterWebRenderer`
  (an approach used in previous releases) displays a
  **deprecation notice** in the JS console. For more information,
  check out [Customizing web app initialization][web-app-init].
{{site.alert.end}}

[web-app-init]: {{site.url}}/platform-integration/web/initialization


## Choosing which option to use

Choose the `auto` option (default) if you are optimizing for download size on
mobile browsers and optimizing for performance on desktop browsers.

Choose the `html` option if you are optimizing download size over performance on
both desktop and mobile browsers.

Choose the `canvaskit` option if you are prioritizing performance and
pixel-perfect consistency on both desktop and mobile browsers.

## Examples

Run in Chrome using the default renderer option (`auto`):

```
flutter run -d chrome
```

Build your app in release mode, using the default (auto) option:

```
flutter build web --release
```

Build your app in release mode, using just the CanvasKit renderer:

```
flutter build web --web-renderer canvaskit --release
```

Run  your app in profile mode using the HTML renderer:

```
flutter run -d chrome --web-renderer html --profile
```

[file an issue]: {{site.repo.flutter}}/issues/new?title=[web]:+%3Cdescribe+issue+here%3E&labels=%E2%98%B8+platform-web&body=Describe+your+issue+and+include+the+command+you%27re+running,+flutter_web%20version,+browser+version
