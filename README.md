
<h1 align="center">ngx-prefetch - Load your Angular application faster.</h1>

<p align="center">
  <img src="https://user-images.githubusercontent.com/86055112/211430605-733e1e4b-e439-4c68-9256-f9f11f6785a2.png" alt="ngx-prefetch logo" width="200px" height="200px"/>
  <br>
  <b>Angular builder for prefetching static resources before loading the application.</b>
  <br><br>
  <img alt="npm" src="https://img.shields.io/npm/dw/@o3r/ngx-prefetch?color=red">
  <img alt="npm (scoped)" src="https://img.shields.io/npm/v/@o3r/ngx-prefetch?color=8B8000">
  <img alt="NPM" src="https://img.shields.io/npm/l/@o3r/ngx-prefetch?color=blue">
</p>

<hr>

## Description
In some cases, it is possible to prefetch the static resources of an application before actually loading the application itself. For example, if the application can be accessed through a static web page or another web application.

The prefetch builder generates a `ngxPrefetch.js` file that should be included in the HTML page of the entry point. When run, it dynamically [creates `<link>` tags](https://developer.mozilla.org/en-US/docs/Web/HTTP/Link_prefetching_FAQ) for each static resource (such as JS and CSS files) so that the browser can prefetch them during idle times. This will improve the Page Load Time of the application when it is accessed for the first time. Then, the browser caching will take over until a new version of the application is deployed or the browser cache is cleared.

## Prerequisites
A prerequisite for the script is to have [Angular Service Worker](https://angular.io/guide/service-worker-intro) enabled as it is using the `ngsw.json` from the production build  folder that is generated by the Angular Service Worker. Therefore, it will be run after the build prod.

## Install

```shell
npm install -D @o3r/ngx-prefetch
```
OR
```shell
ng add @o3r/ngx-prefetch
```
> **NOTE:** This library is following the Angular release cycle. For instance, if you are using Angular 13, use a 13.x version of the library:
>
> ```shell
> npm install -D @o3r/ngx-prefetch@13
> ```
> OR 
> ```shell
> ng add @o3r/ngx-prefetch@13
> ```

## Get started

1. By running the `ng add` command above, the following lines should have been added to the application's `angular.json`:

```json
"generate-prefetch": {
    "builder": "@o3r/ngx-prefetch:run",
    "options": {
        "targetBuild": "app-name:build:production"
    }
},
```

> **NOTE:** Additional configuration can be added to the `angular.json` (builder options are described below with an example of full configuration).

2. Then, add a command to the `package.json` to run `generate-prefetch`:

```json
"generate:prefetch": "ng run app-name:generate-prefetch"
```

3. Run the `ng build` command with the `production` configuration. 
The default configuration of the build command should be `production`, otherwise it should be specified: 

```json
...
"build:prod": "ng build --configuration production",
...
```

4. Run the previously defined `generate:prefetch` command.

## How it works

Please refer to the details on how the ngx-prefetch works [here](docs/HOW_IT_WORKS.md).

## Builder options
  - `targetBuild` **Mandatory** The target build where prefetch should be applied. Used for identifying the `outputPath` of the build.

  - `resourceTypes` An object describing the resource types that should be prefetched. The valid values for the type of content can be found [here](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link#attributes).

  - `crossorigin` Flag that sets the crossorigin attribute on links. If true it will be set for all prefetched resources.

  - `production` Flag for creating a production (minified) version of the js file or a development one.

  - `staticsFullPath` By default the prefetched resources are hosted next to the `ngxPrefetch.js` file, on the same server. 
  If this is not the case, you can configure the full path of the resources that will be prefetched (ex: https://my-web-app.com/path/to/my-app/). 
  It is also possible to set this value at runtime. Instead of setting it in the Builder's options, you can search for `{STATICS_FULL_PATH}` and replace it on the server side in order to inject a path.

### Example of full configuration

[`angular.json`: full configuration]

```json
"generate-prefetch": {
    "builder": "@o3r/ngx-prefetch:run",
    "options": {
        "targetBuild": "my-app:build:production",
        "resourceTypes": {
            "image": ["png", "jpg", "gif"],
            "font": ["eot", "ttf", "woff", "woff2", "svg"],
            "style": ["css"],
            "script": ["js"],
            "document": ["html"]
        },
        "crossorigin": true,
        "production": false,
        "staticsFullPath": "https://my-web-app.com/path/to/my-app/"
    }
}
```
