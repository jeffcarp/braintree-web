braintree-web
=================

A suite of tools for integrating Braintree in the browser.

This is the repo to submit issues if you have any problems or questions about any v.zero JS integration.

Install
=======

```
npm install braintree-web
```

```
bower install braintree-web
```

Basic Usage
===========

For more thorough documentation, visit [the JS SDK docs](https://developers.braintreepayments.com/javascript/sdk/client).

#### Drop-in integration

```html
<div id="payment-form"></div>
```

```javascript
braintree.setup('your-client-token', 'dropin', {
  container: 'payment-form'
});
```

#### Custom integration

```html
<form id="payment-form" action="/your/server/endpoint" method="post">
  <input data-braintree-name="number" value="4111111111111111" />
  <input data-braintree-name="expiration_date" value="10/20" />
  <input type="submit" value="Purchase" />
</form>
```

```javascript
braintree.setup('your-client-token', 'custom', {
  container: 'payment-form'
});
```

#### Advanced integration

```javascript
var client = new braintree.api.Client({
  clientToken: 'your-client-token'
});

client.tokenizeCard({
  number: '4111111111111111',
  expirationDate: '10/20'
}, function (err, nonce) {
  // Send nonce to your server
});
```

Reference (Drop-in or Custom)
=========

`braintree.setup` is the main entry point for Drop-in and Custom. Here is how it works and what you can do with it:

```javascript
braintree.setup(clientToken, integration, options);
```

- `clientToken` - String - generated by the BT client library on your server.
- `integration` - String - either `"dropin"` or `"custom"`.
- `options` - Object - documented below.

### Options

| option | description |
| --- | --- |
| `container` | **required**, String, DOM or jQuery Element - If String, the ID of the element braintree will attach to. If DOM or jQuery Element, the element braintree will attach to. |
| `form` | `String`, custom only - The `name` attribute of the payment form. Defaults to the nearest parent of `container`. |
| `paymentMethodNonceReceived` | `function (event, nonce)` - For intercepting the default form submit. If you provide this callback, the checkout form will not be submitted automatically. |
| `paypal` | `Object` - For configuring PayPal button and modal. See below. |
| `paypal.container` | **required**, `String` or `Element` - The ID or Element you want the PayPal button to bind to. |
| `paypal.paymentMethodNonceInputField` | `String` or `Element` - The id, native DOM element, or jQuery object specifying an input field that the client will write the resulting `paymentMethodNonce` once the customer has authenticated with PayPal. If not provided, an `<input>` will be created and added to the form. |
| `paypal.displayName` | `String` - The name to display as the merchant inside of the PayPal lightbox. Defaults to the company name on your Braintree account. |
| `paypal.onSuccess` | `function` - A callback function, which is fired after the customer successfully authenticates with PayPal via the popup. It's called with the following arguments: nonce (String), email (String), shippingAddress (Object - only returned when shipping addresses are enabled). |
| `paypal.onUnsupported` | `function` - A callback function that is fired if the customer's browser does not support the PayPal checkout experience. |
| `paypal.onCancelled` | `function` - A callback function, which is fired if the customer cancels or logs out without completing PayPal authentication. |
| `paypal.singleUse` | `Boolean` - Defaults to `false`. When set to `true`, triggers one time flow instead of future payment flow. |
| `paypal.locale` | `String` - The locale. |
| `paypal.enableShippingAddress` | `Boolean` - Whether a shipping address selector should be displayed in a future payment flow. Defaults to `false`. |
