---
title: Tutorial - RocketSDK for Retention Marketing

language_tabs: # must be one of https://git.io/vQNgJ

toc_footers:
  - This document is open source
  - Questions? <a href='https://stackoverflow.com/'>Ask on Stack Overflow</a>
  - Have fixes? <a href='https://github.com/RetentionRocket/tutorial-rocketsdk'>Edit the GitHub repo</a>


includes:

search: true
---

# Using RocketSDK for Retention Marketing

**by Daniel Kehoe**

_Last updated 31 October 2018_

## Introduction

> This "code column" contains code examples. Read the adjacent column for explanation and details.

This tutorial shows how to increase sales for an ecommerce site by adding [Retention Rocket](https://www.retentionrocket.com/). You'll learn how to use the Retention Rocket service to reduce cart abandonment.

Retention Rocket provides a RocketSDK JavaScript library that sends ecommerce event data to the Retention Rocket servers. Retention Rocket responds to events such as cart abandonment by sending SMS or Facebook messages to shoppers.

## Overview

We'll show you how to add the Retention Rocket JavaScript library to your website to monitor ecommerce activity. The RocketSDK JavaScript library sends API requests to the Retention Rocket servers to collect and analyze visitor behavior. When Retention Rocket detects revenue-risk behavior such as cart abandonment, the Retention Rocket application intervenes by sending feedback requests or coupon offers via SMS or Facebook Messenger.

The *RocketSDK.js* tracking script is the front end. You can add it to any website. The RocketSDK JavaScript library sends API requests to the Retention Rocket server URL. The Retention Rocket servers (the back end) record the RocketSDK API requests for analysis, intervention, and reporting.

## Scope and Tasks

The purpose of this tutorial is to show you how to integrate the [Retention Rocket](https://www.retentionrocket.com/) service with an ecommerce application. First, you'll learn how to create and configure a Retention Rocket account. Then you'll use a simple test page to see how the RocketSDK JavaScript library sends event data. Then we will build a simple generic website with HTML and CSS and add the RocketSDK JavaScript library so you can see how the library integrates with ecommerce HTML. Finally, we'll trigger RocketSDK events on the simple website and observe the results in the Retention Rocket dashboard.

## Create Your Account

You must create an account at the [Retention Rocket website](https://www.retentionrocket.com/) so the Retention Rocket servers will respond to API requests from your application. Visit the website to get started.

<aside class="warning">
TODO: Provide instructions and screenshots to create and configure a Retention Rocket account, including obtaining an API key.
</aside>

### Obtain Your API Key

Your API key identifies your Retention Rocket account. You'll obtain your API key from your Retention Rocket dashboard. Make a note of it now. You'll need it later when you use the RocketSDK test page and when you set up tracking on your ecommerce site.

## Download the Project

```bash
$ git clone https://github.com/RetentionRocket/rocketSdk
$ cd rocketSdk
$ yarn install
$ yarn build
```

The public [RocketSDK GitHub repository](https://github.com/RetentionRocket/rocketSdk) contains a test page *index.html*.

Copy the project files to your local environment using `git clone`.

`$ git clone https://github.com/RetentionRocket/rocketSdk`

`$ cd rocketSdk`

You're in the RocketSDK project folder.

<aside class="warning">
TODO: The RocketSDK JavaScript file should be available on a CDN so the user doesn't have to build the JavaScript library from the repo before using it.
</aside>

Use [yarn](https://yarnpkg.com/en/) to build the distribution version of the script from the GitHub source files.

`$ yarn install`

`$ yarn build`

You will find a distribution release of the file *rocketSdk.js* in the *dist* folder.

## RocketSDK Test Page

Open the test page *index.html* in a Chrome web browser. It is a single HTML file that mimics an ecommerce website. Links and buttons on the test page open modal windows that resemble a shopping cart and checkout page. You can click the buttons to see the API requests in the JavaScript console.

<aside class="notice">
The Developer Tools view is a diagnostic tool for front-end development. In Chrome on macOS, press Command-Option-I to open the Developer Tools View in a section of the browser window. Alternatively, you can find the menu item under View/Developer/Developer Tools. In Chrome on Windows or Linux platforms, press Shift-Ctrl-I or select Menu/Tools/Developer Tools. Click the tab "Console" to view the output from the JavaScript console.
</aside>

When you open the JavaScript console in Chrome Developer Tools, you may see an error message:

`POST ... net::ERR_NAME_NOT_RESOLVED`

The error message indicates the Retention Rocket server hostname is not set correctly in the *index.html* file. If you see the error message:

` Failed to load resource: the server responded with a status of 404 (Not Found)`

you will know the server is responding but it is not set up correctly to receive an API request.

Even if you see error messages in the console, you can still see the API requests sent by the test page.

<aside class="warning">
TODO: If our API server is running, we can remove the section above (about error messages).
</aside>

You can view API requests in Chrome Developer Tools window. Click the "Network" tab in the Developer Tools window and filter for the requests named `events`. You can see the Request URL, the Response Headers, the Request Headers, and the Form Data. Every page refresh or click on the page will generate a new event that you can see in Chrome Developer Tools.

If you have done nothing other than viewing the page, you will only see one event. It will contain form data notifying Retention Rocket that you've viewed the "RocketSDK Test Page."

### Set the API Key

```html
# index.html
<!-- Retention Rocket -->
<script src="dist/rocketSdk.js" apiKey='abc123'></script>
<script>
  rocketSdk.configure({
    hostname: 'http://api.retentionrocket.com',
  });
  rocketSdk.debug();
</script>
```

Open the file *index.html*. Earlier you saw how to obtain your API key from your Retention Rocket dashboard. Replace the API key `abc123` with your own.

`apiKey='abc123'`

<aside class="warning">
TODO: I'm calling it "apiKey" but we haven't changed the RocketSDK library yet to call it apiKey instead of sdkToken.
</aside>

Setting the API key allows Retention Rocket to record and display ecommerce events in your Retention Rocket dashboard. We are interested in five ecommerce events:

* Identify Customer
* Product Viewed
* Product Added
* Checkout Started
* Order Completed

We'll use the RocketSDK test page to send these events to Retention Rocket. API requests sent to Retention Rocket conform to a standard [Ecommerce Events specification](https://segment.com/docs/spec/ecommerce/v2/) established by [Segment.com](https://segment.com/).

### Customer Identity Form

At the top of the test page you'll see a simple form that asks the visitor to enter a name, email address and phone number. For retention marketing to work, Retention Rocket has to receive a shopper's phone number. You may already be asking a shopper for their name and email address, in which case it's easy to ask for a phone number in the same form. If Retention Rocket receives a shopper's phone number before a final checkout event, Retention Rocket can send an SMS message to encourage a shopper to complete their order if they leave the site before placing an order.

### Opt-in Checkbox

You can ask a shopper for a phone number at any point in their shopping experience. When you ask for a phone number, you should also offer the shopper an option to opt-in to "Receive offers via text message." The checkbox should be accompanied by a legal notice that states clearly that providing the phone number and opting in indicates the shopper's consent to receive automated marketing messages via SMS. Retention Rocket only sends SMS messages to shoppers if they have agreed to receive offers via text message.

### Identity Event

When your customer submits a form that contains their phone number and opt-in choice, your web page should send the information to Retention Rocket as an "Identity Event" API request. It is not necessary to send the Identity event before products are viewed or added to a cart. The RocketSDK script can capture and send information about shopping behavior before a customer creates an account or submits an order. Sending an identity event allows Retention Rocket to associate shopping behavior such as items added to a cart with a shopper's phone number for follow-on marketing.

 Try sending an "Identify" API request from the test page. Fill out the form and click the "Create Account" button. Clicking the button will send an "Identify" API request to the Retention Rocket server. If you watch the events carefully in the Developer Tools "Network" tab, you'll see a quick flash after you click the button as the event is sent. The "Identify" event quickly will be replaced by the web page refreshing after the form submission.

 <aside class="warning">
 TODO: It kinda sucks that Developer Tools refreshes after the form is submitted (and we can't see the form submission event). Maybe there is a way to display the form submission event that I don't know?
 </aside>

### View Product

The test page contains a link "Item One" below the first product image. Try clicking the link "Item One." A modal window will open with a product image. If you check the Developer Tools "Network" tab, you'll see a new event. Under the "Form Data" heading, you'll see the event name "Product Viewed" and properties that describe the product.

### Add to Cart

The "View Product" modal window contains a button "Add to Cart." Clicking the "Add to Cart" button will open a modal window showing the contents of the cart. Clicking the button sends a "Product Added" API request. You can see the event in the Developer Tools window.  Under the "Form Data" heading for the newest event, you'll see the event name "Product Added" and properties that describe the product.

### Checkout

The "Products in Cart" modal window contains a button "Checkout." Clicking the "Checkout" button will open a modal window titled "Your Order." Clicking the button sends a "Checkout Started" API request. You can see the event in the Developer Tools window.  Under the "Form Data" heading for the newest event, you'll see the event name "Checkout Started" and properties that describe the order.

### Order Now

The "Your Order" modal window contains a button "Order Now." Clicking the "Order Now" button will open a modal window titled "Thank You." Clicking the button sends an "Order Completed" API request. You can see the event in the Developer Tools window. Under the "Form Data" heading for the newest event, you'll see the event name "Order Completed" and properties that describe the order.

### Ecommerce Events

We recommend that you use the RocketSDK test page to confirm your account is set up properly and events are received by Retention Rocket.

<aside class="warning">
TODO: After the Retention Rocket API server is set up, add instructions to check if the events are displayed in the Retention Rocket dashboard.
</aside>

Next we'll investigate how the RocketSDK test page works so you'll be able to integrate the RocketSDK JavaScript library with your own site. To understand how the RocketSDK test page works, we'll build it from scratch so you can see how the RocketSDK JavaScript library integrates with a typical ecommerce web page.

## Simple Ecommerce Website

We'll start by downloading a Bootstrap template from the [Start Bootstrap](https://startbootstrap.com/) site to save effort and get a web page that looks like a typical ecommerce website.

Visit the [Shop Homepage](https://startbootstrap.com/template-overviews/shop-homepage/) and click the download button to get a zip archive file on your local machine. Open the zip archive and rename the folder to "Rocket Shop."

### Install the RocketSDK JavaScript Library

```html
# index.html
<head>

  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <meta name="description" content="">
  <meta name="author" content="">

  <title>Shop Homepage - Start Bootstrap Template</title>

  <!-- Bootstrap core CSS -->
  <link href="vendor/bootstrap/css/bootstrap.min.css" rel="stylesheet">

  <!-- Custom styles for this template -->
  <link href="css/shop-homepage.css" rel="stylesheet">

  <!-- Retention Rocket -->
  <script src="vendor/rocketSdk.js" apiKey='abc123'></script>

</head>
```

You've already downloaded the RocketSDK JavaScript library from the [RocketSDK GitHub repository](https://github.com/RetentionRocket/rocketSdk). You've already built the *RocketSdk.js* file and it should be in the *dist* folder. Copy and save the file in the Rocket Shop folder *vendor* with the name *RocketSdk.js*.

<aside class="warning">
TODO: The RocketSDK JavaScript file should be available on a CDN so the user doesn't have to build the JavaScript library from the repo before using it.
</aside>

Inside the Rocket Shop folder is an *index.html* file. Open the file in a text editor and edit the `<head>` section. Add a line before the final `</head>` tag:

`<script src="vendor/rocketSdk.js" apiKey='abc123'></script>`

This directive will include the RocketSDK JavaScript library from the *vendor* folder. Earlier you obtained Retention Rocket API key from your Retention Rocket dashboard. You should set `apiKey` with your Retention Rocket API key (replace `abc123` with your API key).

View the web page by opening the *index.html* file in a web browser. Use Chrome Developer Tools to verify that the *RocketSdk.js* file is loaded (it will appear under the Sources and Network tabs).

### Test the RocketSDK JavaScript Library

```console
> rocketSdk.debug();
< true
```

Switch to the Console tab in Developer Tools to test the RocketSDK JavaScript library. Enter the `rocketSdk.debug()` directive to test if the RocketSDK library responds.

`rocketSdk.debug();`

The console will display `true` if the library is installed correctly.

### Add the Customer Identity Form

```html
<div class="form-container my-4 card">
  <form data-rr-form class="card-body">
    <div class="form-row">
      <div class="col">
        <label for="rr-firstName">First Name</label><br>
        <input type="text" id="rr-firstName" class="form-control" placeholder="Your first name" data-rr='firstName'>
      </div>
      <div class="col">
        <label for="rr-lastName">Last Name</label><br>
        <input type="text" id="rr-lastName" class="form-control" placeholder="Your last name" data-rr='lastName'>
      </div>
    </div>
    <div class="form-row mt-2">
      <div class="col">
        <label for="rr-email">Email Address</label><br>
        <input type="email" id="rr-email" class="form-control" placeholder="Your email" data-rr='email'>
      </div>
      <div class="col">
        <label for="rr-phone">Phone Number</label><br>
        <input type="text" id="rr-phone" class="form-control" placeholder="Your phone number" data-rr='phone'>
      </div>
    </div>
    <div>
      <input type="checkbox" data-rr='smsOptin' class="mt-4 ml-4"><label class="ml-2">Receive offers via text message</label>
      <div class="small ml-4">By checking this box I consent to receive automated marketing by text message an automatic telephone dialing system. Consent is not a condition to purchase.</div>
    </div>
    <div class="mt-2 text-center">
      <button type="submit" class="btn btn-primary btn-lg">Create Account</button>
    </div>
  </form>
</div>
```

Let's replace the image carousel at the top of the Shop Homepage with a customer identity form.

The Shop Homepage contains a `div` with the ID `carouselExampleIndicators`.

`<div id="carouselExampleIndicators" class="carousel slide my-4" data-ride="carousel">
.
.
.
</div>`

Replace it with the form code from the RocketSDK test file *index.html*.

`<div class="form-container my-4 card">
.
.
.
</div>`

Refresh the web page in the browser and you'll see the identity form instead of the image carousel. Fill in the form and click the "Create Account" button. The behavior will be the same as you saw with the RocketSDK test page. The Shop Homepage will send an "Identify" event to Retention Rocket. It is difficult to see in the Developer Tools window because the Developer Tools window refreshes quickly after the event is sent.

Let's see how the RocketSDK library recognizes the form submission and obtains the shopper identity information to send to Retention Rocket.

`<form data-rr-form class="card-body">`

The `<form>` tag contains an HTML5 data attribute `data-rr-form`. The RocketSDK library contains a convenience function `trackIdentify` that recognizes when a user submits any form that contains the `data-rr-form` data attribute in the `<form>` tag. When the submit button is clicked, the convenience function looks for `<input>` tags that contain data attributes:

* `data-rr='firstName'``
* `data-rr='lastName'`
* `data-rr='email'`
* `data-rr='phone'`
* `data-rr='smsOptin'`

If these data attributes are present in a form, the RocketSDK library will send the corresponding information in an "Identify" event API request. To send an "Identify" event from your own ecommerce site, add the same data attributes to a `<form>` tag and the form's `<input>` tags.

The RocketSDK function `trackIdentify` is purely a convenience function that makes it easy to send an identity event from an existing form by merely adding data attributes to the `<form>` tag and  `<input>` tags. In most cases, this is the easiest way to update your site to send identity information to Retention Rocket. If it is not practical to add data attributes to your forms, a developer can write custom JavaScript to use an `EventListener` to recognize a form submission and query the DOM to obtain information to send as an "Identify" event.

Next we'll look at writing custom JavaScript using an `EventListener` to send additional ecommerce events:

* Product Viewed
* Product Added
* Checkout Started
* Order Completed

It is not practical to provide convenience methods to capture these events. Instead, you must use custom JavaScript to listen for link or button clicks and obtain product information.

### Custom JavaScript

```js
<script>
  if(document.readyState === 'complete') {
    rrTracking();
  } else {
    document.addEventListener('DOMContentLoaded', function () {
      rrTracking();
    });
  }
  // replace the rrTracking function with useful code
  function rrTracking() {
    // event listeners go here
  };
</script>
```

Open the Rocket Shop *index.html* file in a text editor and edit the `<head>` section. You've already added the `<script>` tag to include the RocketSDK library.

`<script src="vendor/rocketSdk.js" apiKey='abc123'></script>`

Add additional JavaScript code before the final `</head>` tag. You'll add a function `rrTracking()` to contain event listeners. But first you must assure that the page is fully loaded before the function `rrTracking()` is initiated.

### "Product Viewed" JavaScript

```js
// replace the rrTracking function with useful code
function rrTracking() {
  // track 'Product Viewed'
  let properties_product_viewed = {
    // example properties (set dynamically in production)
    product_id: '1',
    name: 'Item One'
  };
  document.getElementById("product-viewed-link").addEventListener("click",
  function(){rocketSdk.track('Product Viewed', properties_product_viewed);});
  // track 'Product Added'
  let properties_product_added = {
    // example properties (set dynamically in production)
    product_id: '1',
    name: 'Item One'
  };
  document.getElementById("product-added-button-1").addEventListener("click",
  function(){rocketSdk.track('Product Added', properties_product_added);});
  document.getElementById("product-added-button-2").addEventListener("click",
  function(){rocketSdk.track('Product Added', properties_product_added);});
  // track 'Checkout Started'
  let properties_checkout_started = {
    // example properties (set dynamically in production)
    order_id: '1',
    value: 0,
    products: [
      {
        product_id: '1',
        name: 'Item One',
        price: 0,
        quantity: 1
      }
    ]
  };
  document.getElementById("checkout-button").addEventListener("click",
  function(){rocketSdk.track('Checkout Started', properties_checkout_started);});
  // track 'Order Completed'
  let properties_order_completed = {
    // example properties (set dynamically in production)
    order_id: '1',
    value: 0,
    products: [
      {
        product_id: '1',
        name: 'Item One',
        price: 0,
        quantity: 1
      }
    ]
  };
  document.getElementById("order-button").addEventListener("click",
  function(){rocketSdk.track('Order Completed', properties_order_completed);});
  };
```

Replace the empty function `rrTracking()` with new code. The new code adds an event listener to track "Product Viewed" events. First you'll create a hash `properties_product_viewed` to provide product information to Retention Rocket. You'll obtain an element with an ID `product-viewed-link` from the DOM. Add an event listener with `addEventListener`. Listen for a click. Then call an anonymous function that sends the `rocketSdk.track` API request. The API request requires a name "Product Viewed" and accepts a hash of properties that describe the product.

<aside class="warning">
TODO: We need to show how to set the properties using product information from the web page.
</aside>

The Segment [Ecommerce Events specification](https://segment.com/docs/spec/ecommerce/v2/#core-ordering) allows over a dozen properties for a "Product Viewed" event. Retention Rocket requires only a `product_id` property at minimum. However, you can supply optional properties (shown here with examples):

 * product_id: '507f1f77bcf86cd799439011',
 * sku: 'G-32',
 * category: 'Games',
 * name: 'Monopoly: 3rd Edition',
 * brand: 'Hasbro',
 * variant: '200 pieces',
 * price: 18.99,
 * quantity: 1,
 * coupon: 'MAYDEALS',
 * currency: 'usd',
 * position: 3,
 * value: 18.99,
 * url: 'https://www.example.com/product/path',
 * image_url: 'https://www.example.com/product/path.jpg'

### Add "Product Viewed" ID

In the Rocket Shop *index.html* file, find the link that contains the "Item One" anchor text and add the attribute `id="product-viewed-link"`.

`<a id="product-viewed-link" data-toggle="modal" data-target="#modal-a" href="#" data-dismiss="modal">Item One</a>`

The event listener will recognize a click on the link with this ID.

### "Product Added" JavaScript

The event listener for "Product Added" events is very similar. Retention Rocket requires only a `product_id` property at minimum. However, you can supply optional properties (shown here with examples):

* product_id: '507f1f77bcf86cd799439011',
* sku: 'G-32',
* category: 'Games',
* name: 'Monopoly: 3rd Edition',
* brand: 'Hasbro',
* variant: '200 pieces',
* price: 18.99,
* quantity: 1,
* coupon: 'MAYDEALS',
* currency: 'usd',
* position: 3,
* value: 18.99,
* url: 'https://www.example.com/product/path',
* image_url: 'https://www.example.com/product/path.jpg'

### Add "Product Added" ID

In the Rocket Shop *index.html* file, find a button that contains the "Add to Cart" text and add the attribute `id="product-added-button-1"`.

Find another button that contains the "Add to Cart" text and add the attribute `id="product-added-button-2"`.

`<button id="product-added-button-1" type="button" class="btn btn-primary btn-sm" data-toggle="modal" data-target="#modal-b">Add to Cart</button>`

`<button type="button" id="product-added-button-2" class="btn btn-primary" data-toggle="modal" data-target="#modal-b" data-dismiss="modal">Add to Cart</button>`

The event listener will recognize a click on the buttons with these IDs.

### "Checkout Started" JavaScript

The event listener for "Checkout Started" events is very similar. Retention Rocket requires only a `order_id` or `checkout_id` property at minimum. However, you can supply optional properties (shown here with examples):

* checkout_id: 'fksdjfsdjfisjf9sdfjsd9f',
* order_id: '50314b8e9bcf000000000000',
* affiliation: 'Google Store',
* value: 30,
* revenue: 25,
* shipping: 3,
* tax: 2,
* discount: 2.5,
* coupon: 'hasbros',
* currency: 'USD'

Additionally you can supply an array of hashes of product properties.

### Add "Checkout Started" ID

In the Rocket Shop *index.html* file, find the button with the "Checkout" text and add the attribute `id="checkout-button"`.

`button type="button" id="checkout-button" class="btn btn-primary" data-toggle="modal" data-target="#modal-c" data-dismiss="modal">Checkout</button>`

The event listener will recognize a click on the link with this ID.

### "Order Completed" JavaScript

The event listener for "Order Completed" events is very similar. Retention Rocket requires only a `order_id` or `checkout_id` property at minimum. The `order_id` or `checkout_id` should be the same value for both "Checkout Started" and "Order Completed" events. You can supply optional properties (shown here with examples):

* checkout_id: 'fksdjfsdjfisjf9sdfjsd9f',
* order_id: '50314b8e9bcf000000000000',
* affiliation: 'Google Store',
* total: 27.50,
* value: 30,
* revenue: 25,
* shipping: 3,
* tax: 2,
* discount: 2.5,
* coupon: 'hasbros',
* currency: 'USD'

Additionally you can supply an array of hashes of product properties.

### Add "Order Completed" ID

In the Rocket Shop *index.html* file, find the button with the "Order Now" text and add the attribute `id="order-button"`.

`<button type="submit" id="order-button" class="btn btn-primary" data-toggle="modal" data-target="#modal-d">Order Now</button>`

The event listener will recognize a click on the link with this ID.

## Events in Retention Rocket

You've seen how to integrate the RocketSDL library with a simple ecommerce website and listen for ecommerce events to send to Retention Rocket. Every ecommerce website is different but with this understanding, you should be able to add the RocketSDL library to your own online shop and connect to Retention Rocket. You've seen how to trigger RocketSDK events on the simple website and observe the results in the Retention Rocket dashboard. With a successful integration, you'll see your own ecommerce events in the Retention Rocket dashboard.

<aside class="warning">
TODO: Add something more about using Retention Rocket.
</aside>
