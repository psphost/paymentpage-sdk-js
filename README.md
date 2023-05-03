# PSPHost Javascript package

### What is it?

It is package that will help you with generating payment URL according to 
[PSPHost documentation](https://developers.psphost.kz/en/en_PP_Integration.html).

### How to use?

#### Get payment page URL

1. Install the package (with your package manager):
```shell
npm install psphost
yarn add psphost
```

2. Require somewhere in your code, set parameters and get the URL:
```javascript
const { Payment } = require('psphost');

// create psphost object with your account ID and secret salt
const e = new Payment('112', 'my_secret');

// set payment details 
e.paymentAmount = 1000;
e.paymentId = 'FFCD12-30';
e.paymentCurrency = 'USD';

// set another parameters, like success or fail callback URL, customer details, etc.

// get payment URL
const url = e.getUrl();
```

Now your can render payment `url` somewhere on your checkout page.

#### Receive callback from PSPHost

Example with [Express](http://expressjs.com):
```javascript
const { Callback } = require('psphost');

app.post('/payment/callback', function(req, res) {
  const callback = new Callback('secret', req.body);
  if (callback.isPaymentSuccess()) {
    const paymentId = callback.getPaymentId();
    // here is your code for success payment
  }
});
```
Note that `Callback` constructor throws Error if signature isn't valid.
