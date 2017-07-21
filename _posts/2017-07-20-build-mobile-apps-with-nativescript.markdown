---
layout: post
title: "Build Mobile Apps With NativeScript"
description: Learn how to quickly build cross-platform mobile apps with NativeScript.
date: 2017-07-2108:30
category: Technical Guide, Mobile, NativeScript
author:
  name: Prosper Otemuyiwa
  url: https://twitter.com/unicodeveloper?lang=en
  avatar: https://en.gravatar.com/avatar/1097492785caf9ffeebffeb624202d8f?s=200
  mail: prosper.otemuyiwa@auth0.com
design:
  bg_color: "#222425"
  image: https://cdn.auth0.com/blog/blog/React-logo.png
tags:
- reactjs
- redux
- javascript
- authentication
- web-app
- auth0
related:
- 2015-04-09-adding-authentication-to-your-react-flux-app
- 2016-01-04-secure-your-react-and-redux-app-with-jwt-authentication
- 2016-09-29-angular-2-authentication
---

---

**TL;DR:** NativeScript is an open-source framework for building truely native iOS and Android apps with JavaScript. It allows you to deploy mobile apps for different platforms with a single code base. write code once and deploy across different platforms. Currently, NativeScript has over 10,000 stars on [GitHub](https://github.com/NativeScript/NativeScript). In this tutorial, I'll show you the power of NativeScript and build a mobile application along side. Check out the [repo](https://github.com/auth0-blog/nativescript-authentication-tutorial) to get the code.

---

## Introduction to NativeScript

**NativeScript** is a JavaScript framework built and maintained by [Progress Software Corporation](https://www.progress.com/). It allows you build truly native mobile apps. It allows you develop truly mobile apps with the web language you are familiar with. You can use TypeScript, Angular and vanilla JavaScript with NativeScript to build mobile apps. NativeScript gives your app a native-like performance by rendering the UI with the native platform's rendering engine instead of webviews.

NativeScript runtime gives you the power to access native APIs and SDKs when you need them. In addition, it has a burgeoning community of users and a comprehensive [documentation](http://docs.nativescript.org).

## Key Components and Concepts in NativeScript

You need to understand the key concepts needed in building mobile apps with NativeScript.

### User Interface

Handling user interfaces the right way is very important in every application including mobile apps. NativeScript ships with some user interface widgets such as Button, Label, TextField, SearchBar, Slider, Progress, ActivityIndicator, Image, DatePicker, TimePicker and [so many others](http://docs.nativescript.org/ui/components).

For example, a search bar widget is `android.widget.SearchView` for Android and `UISearchBar` for iOS.

![SearchBar](http://docs.nativescript.org/img/gallery/android/searchBarPage.png)

Furthermore, NativeScript ships with user interface layouts for positioning views on the mobile screen. Check out some of the predefined layouts below:

![Flexbox Layout](http://docs.nativescript.org/img/gallery/android/flexboxLayoutPage.png)
_FlexboxLayout_

![Dock Layout](http://docs.nativescript.org/cookbook/ui/layouts/dock-layout)
_DockLayout_

![Stack Layout](http://docs.nativescript.org/cookbook/ui/layouts/stack-layout)
_StackLayout_

![Grid Layout](http://docs.nativescript.org/cookbook/ui/layouts/grid-layout)
_GridLayout_

Check out for more [UI layouts](http://docs.nativescript.org/ui/layouts).

### Application LifeCycle

Building an application in NativeScript requires you to understand how screens work, transition of data from one screen to another, events propagation, accessing native APIs amongst other things.

In NativeScript, apps are collection of pages and each page represents different application screens. These pages are objects of the `page` class of the `Page` module and navigating between pages will involve using methods of the `Frame` class of the `Frame` module. You can create your page layout in XML, JavaScript or TypeScript. If you are creating it with XML, then you'll have to create another file for the code responsible for managing the page.

```xml
  <!-- main-page.xml-->
  <Page loaded="onPageLoaded">
    <Label text="Hello, world!"/>
  </Page>
```
_Creating the Page with XML_

```js
var pagesModule = require("ui/page");
var labelModule = require("ui/label");
function createPage() {
    var label = new labelModule.Label();
    label.text = "Hello, world!";
    var page = new pagesModule.Page();
    page.content = label;
    return page;
}
exports.createPage = createPage;
```
_Creating the Page with JavaScript_


* **Event Handling**: NativeScript makes the *Observable* class available that coordinates working with events. NativeScript provides short and full syntax for adding event handlers and listeners. Check out the following example below:

    ```js
    //Adding a listener with the short syntax
    var buttonModule = require("ui/button");
    var applyButton = new buttonModule.Button();
    applyButton.text = "Apply Now!";

    applyButton.on(buttonModule.Button.tapEvent, function (eventData) {
      console.log("You just clicked the apply button!");
    }, this);

    //Adding a lister with the full syntax
    var registerButton = new buttonModule.Button();
    registerButton.text = "Register";

    var onTap = function (eventData) {
      console.log("Register a Student");
    };

    registerButton.addEventListener(buttonModule.Button.tapEvent, onTap, this);

    //Removing a listener with short syntax
    applyButton.off(buttonModule.Button.tapEvent);

    //Removing a listener with full syntax
    applyButton.removeEventListener(buttonModule.Button.tapEvent);
    ```
    _Using JavaScript_

You can use XML but you'll need a different file with the same file name and a `.js` extension.

    ```xml
    <Page>
      <DockLayout>
        <Button tap="onTap" />
      </DockLayout>
    </Page>
    ```

    _Using XML_

* **Data Binding**: In NativeScript, how do you connect the UI to the code? NativeScript supports one-way and two-way data binding. And all NativeScript UI components are created with data binding in mind by default.

    _Two way data binding with JavaScript_

    ```js
    var observableModule = require("data/observable");
    var source = new observableModule.Observable();
    source.textSource = "Let's implement two way data binding";

    //create the TextField
    var textFieldModule = require("ui/text-field");
    var targetTextField = new textFieldModule.TextField();

    //binding the TextField
    var textFieldBindingOptions = {
        sourceProperty: "textSource",
        targetProperty: "text",
        twoWay: true
    };

    targetTextField.bind(textFieldBindingOptions, source);
    ```

It is quite simple with XML. The binding is done via a mustache syntax.

    ```xml
      <Page>
        <StackLayout>
            <TextField text="{{ textSource }}" />
        </StackLayout>
      </Page>
    ```

* **Accessing Native APIs**: As mentioned earlier in this article, NativeScript allows you access all native APIs from the platform the app is running on. For example, JavaScript arrays are specialized Java arrays on Android and `NSArray` on iOS.

```js
// iOS
// nsArray is not a JavaScript array but a JavaScript wrapper around a native NSArray
var nsArray = NSArray.arrayWithArray(['Four', 'Five', 'Two', 'Seven']);
var jsArray = ['One', 'Two', 'Three']; // pure JavaScript array
var firstCommon = nsArray.firstObjectCommonWithArray(jsArray);
console.log(firstCommon); // Two
```

**NativeScript** consists of a number of open source components you should be aware of as a developer.

* **[iOS runtime](https://github.com/NativeScript/ios-runtime)**: This is the iOS runtime for NativeScript. It allows JavaScript code to be executed on iOs devices.

* **[Android runtime](https://github.com/NativeScript/android-runtime)**: This is the Android runtime for NativeScript. It allows JavaScript code to be executed on Android devices.

* **[NativeScript CLI](https://github.com/NativeScript/nativescript-cli)**: This CLI is written in TypeScript. The NativeScript command-line interface allows you create, build and run mobile apps.

* **Cross-platform Modules**: These modules combine iOS and Android APIs into a uniform JavaScript API.

## Let's Get Started

Without much ado, let's get started with building a mobile app with NativeScript. We'll build a simple app.