# Integrate other payment methods

With Checkout API of Mercado Pago, you can add **alternative payment methods for your customers to make their payments**.


----[mla]----

## How does it work?

To receive other payment methods, you should take into account two aspects:

1. First, you need a frontend to collect customer's e-mail and document, amount and payment method.
1. Then, you need a backend that takes the payment data and can confirm and make the payment.

Both for frontend and backend, we recommend [our libraries](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/previous-requirements/#bookmark_always_use_our_libraries) to collect user sensitive data securely.

## Check the available payment methods

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Payment Methods

In addition to cards, you can offer other payment choices on your website.

| Type of payment method | Payment method |
| --- | --- |
| `ticket` | Rapipago |
| `ticket` | Pago Fácil |
| `ticket` | Provincia NET Pagos |
| `ticket` | Carga Virtual |
| `ticket` | Cobro Express |
| `atm` | Red Link |

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Obtain the available payment methods

You can check the available payment methods whenever you need.

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment_methods = MercadoPago::get("/v1/payment_methods");

?>
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

payment_methods = mercadopago.get("/v1/payment_methods");
```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

payment_methods = MercadoPago.SDK.get("/v1/payment_methods");

```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_methods_response = sdk.payment_methods.get()
payment_methods = payment_methods_response[:response]

```
```csharp
using MercadoPago.Client.PaymentMethod;
using MercadoPago.Config;
using MercadoPago.Resource;
using MercadoPago.Resource.PaymentMethod;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var client = new PaymentMethodClient();
ResourcesList<PaymentMethod> paymentMethods = await client.ListAsync();

```
```python
import mercadopago
sdk = mercadopago.SDK("ACCESS_TOKEN")

payment_methods_response = sdk.payment_methods().list_all()
payment_methods = payment_methods_response["response"]
```
```curl
curl -X GET \
    -H 'accept: application/json' \
    -H 'content-type: application/json' \
    -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
    'https://api.mercadopago.com/v1/payment_methods' \
```
]]]

<br>

The result will be a list of payment methods and their features. For example, `payment_type_id` payment methods with `ticket` as value refer to cash payment method.

Keep in mind that the answer will return all the payments methods. For this reason, you have to filter the options you want to offer according to the [list of available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/other-payment-ways#bookmark_payment_methods).

```json
[
    {
        "id": "rapipago",
        "name": "Rapipago",
        "payment_type_id": "ticket",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/2002.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/2002.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": []
    },
    {
        "id": "redlink",
        "name": "RedLink",
        "payment_type_id": "atm",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/2003.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/2003.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": []
    },
    {
        "...": "..."
    }
]
```

> For more information, check [API References](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/reference).

<br>
<span></span>

> CLIENT_SIDE
>
> h2
>
> Data capture for payment

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Use MercadoPago.js library
**Remember to use our official library to access Mercado Pago API** from your application and collect data securely.

```html
<script src="https://secure.mlstatic.com/sdk/javascript/v1/mercadopago.js"></script>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Add payment form

To capture sensitive data from your customer, **please use our form with the corresponding attributes,** which ensures information security.

You can easily include anything you need and add your own style.

Use the list you consulted in [Obtain the available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-api/other-payment-ways#bookmark_obtain_the_available_payment_methods) to create the payment options you want to offer.

```html
<form action="/process_payment" method="post" id="paymentForm">
    <h3>Payment method</h3>
    <div>
      <select class="form-control" id="paymentMethod" name="paymentMethod">
        <option>Select a payment form</option>

        <!-- Create an option for each payment method with their name and complete the ID in the attribute 'value'. -->
        <option value="--PaymentTypeId--">--PaymentTypeName--</option>
      </select>
    </div>
    <h3>Buyer Details</h3>
    <div>
    <div>
        <label for="payerFirstName">Name</label>
        <input id="payerFirstName" name="payerFirstName" type="text" value="Nome"></select>
      </div>
      <div>
        <label for="payerLastName">Surname</label>
        <input id="payerLastName" name="payerLastName" type="text" value="Sobrenome"></select>
      </div>
      <div>
        <label for="payerEmail">E-mail</label>
        <input id="payerEmail" name="payerEmail" type="text" value="test@test.com"></select>
      </div>
      <div>
        <label for="docType">Document Type</label>
        <select id="docType" name="docType" data-checkout="docType" type="text"></select>
      </div>
      <div>
        <label for="docNumber">Document Number</label>
        <input id="docNumber" name="docNumber" data-checkout="docNumber" type="text"/>
      </div>
    </div>

    <div>
      <div>
        <input type="hidden" name="transactionAmount" id="transactionAmount" value="100" />
        <input type="hidden" name="productDescription" id="productDescription" value="Product name" />
        <br>
        <button type="submit">Pay</button>
        <br>
      </div>
  </div>
</form>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. Configure your public key

Add your [public key]([FAKER][CREDENTIALS][URL]) like this:

```javascript
window.Mercadopago.setPublishableKey("YOUR_PUBLIC_KEY");
```

>  If you still don't have an account to check your credentials, [sign in](https://www.mercadopago[FAKER][URL][DOMAIN]/registration-mp).

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4. Get data for your form

#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Get document types

Document type is one of the mandatory fields. Use the document list to fill out your data.

When you call the next function, MercadoPago.js will automatically fill out the available options, including the _select_ type element with `id = docType` in the form:

```javascript
window.Mercadopago.getIdentificationTypes();
```

> Find more information in the [Document type section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/identification-types).

<br>
<span></span>

> SERVER_SIDE
>
> h2
>
> Payment submission to Mercado Pago

To receive cash payments, just send your customer's e-mail and document, amount and payment method.

Once the request –with all the collected information– is in your backend, it should be submitted to Mercado Pago through our APIs.

For this to work, you should configure your [private key]([FAKER][CREDENTIALS][URL]).

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment = new MercadoPago\Payment();
  $payment->transaction_amount = 100;
  $payment->description = "Product Title";
  $payment->payment_method_id = "rapipago";
  $payment->payer = array(
    "email" => "test_user_19653727@testuser.com"
  );

  $payment->save();

?>
```
```node

var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

var payment_data = {
  transaction_amount: 100,
  description: 'Product Title',
  payment_method_id: 'rapipago',
  payer: {
    email: 'test_user_3931694@testuser.com'
  }
};

mercadopago.payment.create(payment_data).then(function (data) {

}).catch(function (error) {

});

```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

Payment payment = new Payment();

payment.setTransactionAmount(100f)
      .setDescription("Product Title")
      .setPaymentMethodId("rapipago")
      .setPayer(new Payer("test_user_19653727@testuser.com"));

payment.save();
```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_request = {
  transaction_amount: 100,
  description: 'Product Title',
  payment_method_id: 'rapipago',
  payer: {
    email: 'test_user_19653727@testuser.com',
  }
}

payment_response = sdk.payment.create(payment_request)
payment = payment_response[:response]

```
```csharp

using MercadoPago.Config;
using MercadoPago.Client.Payment;
using MercadoPago.Resource.Payment;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var request = new PaymentCreateRequest
{
    TransactionAmount = 100,
    Description = "Product Title",
    PaymentMethodId = "rapipago",
    Payer = new PaymentPayerRequest
    {
        Email = "test_user_19653727@testuser.com",
    },
};

var client = new PaymentClient();
Payment payment = await client.CreateAsync(request);

```
```python
import mercadopago
sdk = mercadopago.SDK("ENV_ACCESS_TOKEN")

payment_data = {
    "transaction_amount": 100,
    "description": "Product Title",
    "payment_method_id": "rapipago",
    "payer": {
        "email": "test_user_19653727@testuser.com"
    }
}

payment_response = sdk.payment().create(payment_data)
payment = payment_response["response"]
```
```curl
curl -X POST \
  'https://api.mercadopago.com/v1/payments' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
  -d '{
  transaction_amount: 100,
  description: "Product Title",
  payment_method_id: "rapipago",
  payer: { email: "test_user_19653727@testuser.com" }
}'
```
]]]

<br>
The response will show pending status until the buyer makes the payment. The payment voucher ID is the same as the Mercado Pago transaction ID.

```json
[
 {
    ...,
    "id": 5466310457,
    "status": "pending",
    "status_detail": "pending_waiting_payment",
    ...,
    "transaction_details": {
        "net_received_amount": 0,
        "total_paid_amount": 100,
        "overpaid_amount": 0,
        "external_resource_url": "https://www.mercadopago.com/mla/payments/ticket/helper?payment_id=123456789&payment_method_reference_id= 123456789&caller_id=123456",
        "installment_amount": 0,
        "financial_institution": null,
        "payment_method_reference_id": "1234567890"
    }
 }
]
```

In the `external_resource_url` field you will find an address with payment instructions for your buyer. You can redirect or send him/her the link.

> NOTE
>
> Note
>
> Customers have 3 to 5 days to pay, depending on the payment method. After that, you should cancel it.

## Expiration date of cash payment

If you want, you can change the default due date of a cash payment by sending the `date_of_expiration` field in the payment creation request. The configured date must be between 1 and 30 days from the issue date.

[[[
```php
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

$payment->date_of_expiration = "2020-05-30T23:59:59.000-04:00";
```
```node
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

date_of_expiration: "2020-05-30T23:59:59.000-04:00",
```
```java
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

payment.setDateOfExpiration("2020-05-30T23:59:59.000-04:00")
```
```ruby
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

date_of_expiration: '2020-05-30T23:59:59.000-04:00',
```
```csharp
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

paymentCreateRequest.DateOfExpiration = DateTime.Parse("2020-05-30T23:59:59.000-04:00");
```
```python
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

"date_of_expiration": "2020-05-30T23:59:59.000-04:00"
```
```curl
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

"date_of_expiration": "2020-05-30T23:59:59.000-04:00",
```
]]]

The deadline for approval of the cash payment is between 1 and 2 working days according to the payment method. Therefore, we recommend that you set the due date with at least 3 days to ensure that payment is made.

Check [credit times by payment method](https://www.mercadopago[FAKER][URL][DOMAIN]/ayuda/Medios-de-pago-y-acreditaci-n_221) when configuring.

> WARNING
>
> Important
>
> If the cash payment is paid after the expiration date, the amount will be refunded to the payer's Mercado Pago account.

## Cancel payments

To avoid collection issues, you need to cancel expired payments. Cash payments should be paid within 3 to 5 business days, based on their relevant term.

Take into account that **you can only cancel payments in process or pending status**. If a payment expires after 30 days, the cancellation is automatic and the final status will be cancelled or expired.

For more information, check the [Refunds and Cancellations section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/manage-account/account/cancellations-and-refunds).

## Payment credit times

Each payment method has its own credit times; it is immediate in some cases, while in others, it may take up to 3 business days.

Check [credit times by payment method](https://www.mercadopago[FAKER][URL][DOMAIN]/ayuda/Medios-de-pago-y-acreditaci-n_221) whenever you need to.

------------

----[mlm]----

## How does it work?

To receive other payment methods, you should take into account two aspects:

1. First, you need a frontend to collect customer's e-mail and document, amount and payment method.
1. Then, you need a backend that takes the payment data and can confirm and make the payment.

Both for frontend and backend, we recommend [our libraries](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/previous-requirements/#bookmark_always_use_our_libraries) to collect user sensitive data securely.

## Check the available payment methods

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Payment Methods

In addition to cards, you can offer other payment choices on your website.

| Type of payment method | Payment method |
| --- | --- |
| `ticket` | OXXO |
| `atm` | Citibanamex |
| `atm` | Santander |
| `atm` | BBVA Bancomer |
| `prepaid_card` | Mercado Pago Card |

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Obtain the available payment methods

You can check the available payment methods whenever you need.

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment_methods = MercadoPago::get("/v1/payment_methods");

?>
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

payment_methods = mercadopago.get("/v1/payment_methods");
```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

payment_methods = MercadoPago.SDK.get("/v1/payment_methods");

```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_methods_response = sdk.payment_methods.get()
payment_methods = payment_methods_response[:response]

```
```csharp
using MercadoPago.Client.PaymentMethod;
using MercadoPago.Config;
using MercadoPago.Resource;
using MercadoPago.Resource.PaymentMethod;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var client = new PaymentMethodClient();
ResourcesList<PaymentMethod> paymentMethods = await client.ListAsync();

```
```python
import mercadopago
sdk = mercadopago.SDK("ACCESS_TOKEN")

payment_methods_response = sdk.payment_methods().list_all()
payment_methods = payment_methods_response["response"]
```
```curl
curl -X GET \
    -H 'accept: application/json' \
    -H 'content-type: application/json' \
    -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
    'https://api.mercadopago.com/v1/payment_methods' \
```
]]]

<br>

The result will be a list of payment methods and their features. For example, `payment_type_id` payment methods with `ticket` as value refer to cash payment method.

Keep in mind that the answer will return all the payments methods. For this reason, you have to filter the options you want to offer according to the [list of available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/other-payment-ways#bookmark_payment_methods).

```json
[
  {
        "id": "oxxo",
        "name": "OXXO",
        "payment_type_id": "ticket",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/oxxo.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/oxxo.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": [],
        "min_allowed_amount": 5,
        "max_allowed_amount": 10000,
        "accreditation_time": 2880,
        "financial_institutions": [],
        "processing_modes": [
            "aggregator"
        ]
    },
{
        "id": "banamex",
        "name": "Citibanamex",
        "payment_type_id": "atm",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/banamex.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/banamex.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": [],
        "min_allowed_amount": 5,
        "max_allowed_amount": 40000,
        "accreditation_time": 60,
        "financial_institutions": [],
        "processing_modes": [
            "aggregator"
        ]
    },
{
        "id": "serfin",
        "name": "Santander",
        "payment_type_id": "atm",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/serfin.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/serfin.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": [],
        "min_allowed_amount": 5,
        "max_allowed_amount": 40000,
        "accreditation_time": 60,
        "financial_institutions": [],
        "processing_modes": [
            "aggregator"
        ]
    },
 {
        "id": "bancomer",
        "name": "BBVA Bancomer",
        "payment_type_id": "atm",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/bancomer.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/bancomer.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": [],
        "min_allowed_amount": 10,
        "max_allowed_amount": 40000,
        "accreditation_time": 60,
        "financial_institutions": [],
        "processing_modes": [
            "aggregator"
        ]
    },
{
        "id": "mercadopagocard",
        "name": "Tarjeta MercadoPago",
        "payment_type_id": "prepaid_card",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/mercadopagocard.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/mercadopagocard.gif",
        "deferred_capture": "supported",
        "settings": [
            {
                "card_number": {
                    "validation": "standard",
                    "length": 16
                },
                "bin": {
                    "pattern": "^539978",
                    "installments_pattern": "^539978",
                    "exclusion_pattern": null
                },
                "security_code": {
                    "length": 3,
                    "card_location": "back",
                    "mode": "mandatory"
                }
            }
        ],
        "additional_info_needed": [
            "cardholder_name"
        ],
        "min_allowed_amount": 5,
        "max_allowed_amount": 300000,
        "accreditation_time": 1440,
        "financial_institutions": [],
        "processing_modes": [
            "aggregator"
        ]
    },
]
```

> For more information, check [API References](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/reference).

<br>
<span></span>

> CLIENT_SIDE
>
> h2
>
> Data capture for payment

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Use MercadoPago.js library
**Remember to use our official library to access Mercado Pago API** from your application and collect data securely.

```html
<script src="https://secure.mlstatic.com/sdk/javascript/v1/mercadopago.js"></script>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Add payment form

To capture sensitive data from your customer, **please use our form with the corresponding attributes,** which ensures information security.

You can easily include anything you need and add your own style.

Use the list you consulted in [Obtain the available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-api/other-payment-ways#bookmark_obtain_the_available_payment_methods) to create the payment options you want to offer.

```html
<form action="/process_payment" method="post" id="paymentForm">
    <h3>Payment method</h3>
    <div>
      <select class="form-control" id="paymentMethod" name="paymentMethod">
        <option>Select a payment form</option>

        <!-- Create an option for each payment method with their name and complete the ID in the attribute 'value'. -->
        <option value="--PaymentTypeId--">--PaymentTypeName--</option>
      </select>
    </div>
    <h3>Buyer Details</h3>
    <div>
    <div>
        <label for="payerFirstName">Name</label>
        <input id="payerFirstName" name="payerFirstName" type="text" value="Nome"></select>
      </div>
      <div>
        <label for="payerLastName">Surname</label>
        <input id="payerLastName" name="payerLastName" type="text" value="Sobrenome"></select>
      </div>
      <div>
        <label for="payerEmail">E-mail</label>
        <input id="payerEmail" name="payerEmail" type="text" value="test@test.com"></select>
      </div>
      <div>
        <label for="docType">Document Type</label>
        <select id="docType" name="docType" data-checkout="docType" type="text"></select>
      </div>
      <div>
        <label for="docNumber">Document Number</label>
        <input id="docNumber" name="docNumber" data-checkout="docNumber" type="text"/>
      </div>
    </div>

    <div>
      <div>
        <input type="hidden" name="transactionAmount" id="transactionAmount" value="100" />
        <input type="hidden" name="productDescription" id="productDescription" value="Product name" />
        <br>
        <button type="submit">Pay</button>
        <br>
      </div>
  </div>
</form>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. Configure your public key

Add your [public key]([FAKER][CREDENTIALS][URL]) like this:

```javascript
window.Mercadopago.setPublishableKey("YOUR_PUBLIC_KEY");
```

>  If you still don't have an account to check your credentials, [sign in](https://www.mercadopago[FAKER][URL][DOMAIN]/registration-mp).

<br>
<span></span>

> SERVER_SIDE
>
> h2
>
> Payment submission to Mercado Pago

To receive cash payments, just send your customer's e-mail and document, amount and payment method.

Once the request –with all the collected information– is in your backend, it should be submitted to Mercado Pago through our APIs.

For this to work, you should configure your [private key]([FAKER][CREDENTIALS][URL]).

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment = new MercadoPago\Payment();
  $payment->transaction_amount = 100;
  $payment->description = "Product Title";
  $payment->payment_method_id = "oxxo";
  $payment->payer = array(
    "email" => "test_user_82045343@testuser.com"
  );

  $payment->save();

?>
```
```node

var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

var payment_data = {
  transaction_amount: 100,
  description: 'Product Title',
  payment_method_id: 'oxxo',
  payer: {
    email: 'test_user_82045343@testuser.com'
  }
};

mercadopago.payment.create(payment_data).then(function (data) {

}).catch(function (error) {

});

```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

Payment payment = new Payment();

payment.setTransactionAmount(100f)
      .setDescription("Product Title")
      .setPaymentMethodId("oxxo")
      .setPayer(new Payer("test_user_82045343@testuser.com"));

payment.save();
```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_request = {
  transaction_amount: 100,
  description: 'Product Title',
  payment_method_id: 'oxxo',
  payer: {
    email: 'test_user_82045343@testuser.com',
  }
}

payment_response = sdk.payment.create(payment_request)
payment = payment_response[:response]

```
```csharp

using MercadoPago.Config;
using MercadoPago.Client.Payment;
using MercadoPago.Resource.Payment;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var request = new PaymentCreateRequest
{
    TransactionAmount = 100,
    Description = "Product Title",
    PaymentMethodId = "oxxo",
    Payer = new PaymentPayerRequest
    {
        Email = "test_user_82045343@testuser.com",
    },
};

var client = new PaymentClient();
Payment payment = await client.CreateAsync(request);

```
```python
import mercadopago
sdk = mercadopago.SDK("ENV_ACCESS_TOKEN")

payment_data = {
    "transaction_amount": 100,
    "description": "Product Title",
    "payment_method_id": "oxxo",
    "payer": {
        "email": "test_user_82045343@testuser.com"
    }
}

payment_response = sdk.payment().create(payment_data)
payment = payment_response["response"]
```
```curl
curl -X POST \
  'https://api.mercadopago.com/v1/payments' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
  -d '{
  transaction_amount: 100,
  description: "Product Title",
  payment_method_id: "oxxo",
  payer: { email: "test_user_82045343@testuser.com" }
}'
```
]]]

<br>
The response will show pending status until the buyer makes the payment. The payment voucher ID is the same as the Mercado Pago transaction ID.

```json
[
 {
    ...,
    "id": 5466310457,
    "status": "pending",
    "status_detail": "pending_waiting_payment",
    ...,
    "transaction_details": {
        "payment_method_reference_id": "24304329",
        "verification_code": "882430432923032000100001",
        "net_received_amount": 0,
        "total_paid_amount": 100,
        "overpaid_amount": 0,
        "external_resource_url": "https://www.mercadopago.com/mlm/payments/sandbox/ticket/helper?payment_id=1234&payment_method_reference_id=12345678&caller_id=1234&hash=aaaaaa-bbb-cccc-dddd-eeeeeeee",
        "installment_amount": 0,
        "financial_institution": "",
        "payable_deferral_period": null,
    }
  }
]
```

In the `external_resource_url` field you will find an address with payment instructions for your buyer. You can redirect or send him/her the link.

> NOTE
>
> Note
>
> Customers have 3 to 5 days to pay, depending on the payment method. After that, you should cancel it.

## Cancel payments

To avoid collection issues, you need to cancel expired payments. Cash payments should be paid within 3 to 5 business days, based on their relevant term.

Take into account that **you can only cancel payments in process or pending status**. If a payment expires after 30 days, the cancellation is automatic and the final status will be cancelled or expired.

For more information, check the [Refunds and Cancellations section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/manage-account/account/cancellations-and-refunds).

## Payment credit times

Each payment method has its own credit times; it is immediate in some cases, while in others, it may take up to 3 business days.

Check [credit times by payment method](https://www.mercadopago[FAKER][URL][DOMAIN]/ayuda/Medios-de-pago-y-acreditaci-n_221) whenever you need to.

## Inform customer payment points

Finally, you should share information about the different places where your customers can pay.

| Payment method | Available stores |
| --- | --- |
| OXXO | OXXO
| BBVA Bancomer | 7-Eleven |
| BBVA Bancomer | K |
| BBVA Bancomer | Farmacias del Ahorro |
| BBVA Bancomer | Casa Ley |
| BBVA Bancomer | BBVA Bancomer |
| Citibanamex| Chedraui |
| Citibanamex| Telecomm |
| Citibanamex| Citibanamex |

------------

----[mlu]----

## How does it work?

To receive other payment methods, you should take into account two aspects:

1. First, you need a frontend to collect customer's e-mail and document, amount and payment method.
1. Then, you need a backend that takes the payment data and can confirm and make the payment.

Both for frontend and backend, we recommend [our libraries](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/previous-requirements/#bookmark_always_use_our_libraries) to collect user sensitive data securely.

## Check the available payment methods

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Payment Methods

In addition to cards, you can offer other payment choices on your website.

| Type of payment method | Payment method |
| --- | --- |
| `ticket` | Abitab |
| `ticket` | Redpagos |

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Obtain the available payment methods

You can check the available payment methods whenever you need.

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment_methods = MercadoPago::get("/v1/payment_methods");

?>
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

payment_methods = mercadopago.get("/v1/payment_methods");
```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

payment_methods = MercadoPago.SDK.get("/v1/payment_methods");

```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_methods_response = sdk.payment_methods.get()
payment_methods = payment_methods_response[:response]

```
```csharp
using MercadoPago.Client.PaymentMethod;
using MercadoPago.Config;
using MercadoPago.Resource;
using MercadoPago.Resource.PaymentMethod;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var client = new PaymentMethodClient();
ResourcesList<PaymentMethod> paymentMethods = await client.ListAsync();

```
```python
import mercadopago
sdk = mercadopago.SDK("ACCESS_TOKEN")

payment_methods_response = sdk.payment_methods().list_all()
payment_methods = payment_methods_response["response"]
```
```curl
curl -X GET \
    -H 'accept: application/json' \
    -H 'content-type: application/json' \
    -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
    'https://api.mercadopago.com/v1/payment_methods' \
```
]]]

<br>

The result will be a list of payment methods and their features. For example, `payment_type_id` payment methods with `ticket` as value refer to cash payment method.

Keep in mind that the answer will return all the payments methods. For this reason, you have to filter the options you want to offer according to the [list of available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/other-payment-ways#bookmark_payment_methods).

```json

[
 {
        "id": "abitab",
        "name": "Abitab",
        "payment_type_id": "ticket",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/abitab.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/abitab.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": [
            "identification_number"
        ],
        "min_allowed_amount": 1,
        "max_allowed_amount": 150000,
        "accreditation_time": 0,
        "financial_institutions": [],
        "processing_modes": [
            "aggregator"
        ]
    },
 {
        "id": "redpagos",
        "name": "Redpagos",
        "payment_type_id": "ticket",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/redpagos.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/redpagos.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": [
            "identification_number"
        ],
        "min_allowed_amount": 1,
        "max_allowed_amount": 150000,
        "accreditation_time": 0,
        "financial_institutions": [],
        "processing_modes": [
            "aggregator"
        ]
    }
]
```

> For more information, check [API References](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/reference).

<br>
<span></span>

> CLIENT_SIDE
>
> h2
>
> Data capture for payment

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Use MercadoPago.js library
**Remember to use our official library to access Mercado Pago API** from your application and collect data securely.

```html
<script src="https://secure.mlstatic.com/sdk/javascript/v1/mercadopago.js"></script>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Add payment form

To capture sensitive data from your customer, **please use our form with the corresponding attributes,** which ensures information security.

You can easily include anything you need and add your own style.

Use the list you consulted in [Obtain the available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-api/other-payment-ways#bookmark_obtain_the_available_payment_methods) to create the payment options you want to offer.

```html
<form action="/process_payment" method="post" id="paymentForm">
    <h3>Payment method</h3>
    <div>
      <select class="form-control" id="paymentMethod" name="paymentMethod">
        <option>Select a payment form</option>

        <!-- Create an option for each payment method with their name and complete the ID in the attribute 'value'. -->
        <option value="--PaymentTypeId--">--PaymentTypeName--</option>
      </select>
    </div>
    <h3>Buyer Details</h3>
    <div>
    <div>
        <label for="payerFirstName">Name</label>
        <input id="payerFirstName" name="payerFirstName" type="text" value="Nome"></select>
      </div>
      <div>
        <label for="payerLastName">Surname</label>
        <input id="payerLastName" name="payerLastName" type="text" value="Sobrenome"></select>
      </div>
      <div>
        <label for="payerEmail">E-mail</label>
        <input id="payerEmail" name="payerEmail" type="text" value="test@test.com"></select>
      </div>
      <div>
        <label for="docType">Document Type</label>
        <select id="docType" name="docType" data-checkout="docType" type="text"></select>
      </div>
      <div>
        <label for="docNumber">Document Number</label>
        <input id="docNumber" name="docNumber" data-checkout="docNumber" type="text"/>
      </div>
    </div>

    <div>
      <div>
        <input type="hidden" name="transactionAmount" id="transactionAmount" value="100" />
        <input type="hidden" name="productDescription" id="productDescription" value="Product name" />
        <br>
        <button type="submit">Pay</button>
        <br>
      </div>
  </div>
</form>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. Configure your public key

Add your [public key]([FAKER][CREDENTIALS][URL]) like this:

```javascript
window.Mercadopago.setPublishableKey("YOUR_PUBLIC_KEY");
```

>  If you still don't have an account to check your credentials, [sign in](https://www.mercadopago[FAKER][URL][DOMAIN]/registration-mp).

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4. Get data for your form

#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Get document types

Document type is one of the mandatory fields. Use the document list to fill out your data.

When you call the next function, MercadoPago.js will automatically fill out the available options, including the _select_ type element with `id = docType` in the form:

```javascript
window.Mercadopago.getIdentificationTypes();
```

> Find more information in the [Document type section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/identification-types).

<br>
<span></span>

> SERVER_SIDE
>
> h2
>
> Payment submission to Mercado Pago

To receive cash payments, just send your customer's e-mail and document, amount and payment method.

Once the request –with all the collected information– is in your backend, it should be submitted to Mercado Pago through our APIs.

For this to work, you should configure your [private key]([FAKER][CREDENTIALS][URL]).

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment = new MercadoPago\Payment();
  $payment->transaction_amount = 100;
  $payment->description = "Product Title";
  $payment->payment_method_id = "abitab";
  $payment->payer = array(
    "email" => "test_user_84162205@testuser.com"
  );

  $payment->save();
?>
```
```node

var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

var payment_data = {
  transaction_amount: 100,
  description: 'Product Title',
  payment_method_id: 'abitab',
  payer: {
    email: 'test_user_84162205@testuser.com'
  }
};

mercadopago.payment.create(payment_data).then(function (data) {

}).catch(function (error) {

});

```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

Payment payment = new Payment();

payment.setTransactionAmount(100f)
      .setDescription("Product Title")
      .setPaymentMethodId("abitab")
      .setPayer(new Payer("test_user_84162205@testuser.com"));

payment.save();
```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_request = {
  transaction_amount: 100,
  description: 'Product Title',
  payment_method_id: 'abitab',
  payer: {
    email: 'test_user_84162205@testuser.com',
  }
}

payment_response = sdk.payment.create(payment_request)
payment = payment_response[:response]
```
```csharp

using MercadoPago.Config;
using MercadoPago.Client.Payment;
using MercadoPago.Resource.Payment;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var request = new PaymentCreateRequest
{
    TransactionAmount = 100,
    Description = "Product Title",
    PaymentMethodId = "rapipago",
    Payer = new PaymentPayerRequest
    {
        Email = "test_user_84162205@testuser.com",
    },
};

var client = new PaymentClient();
Payment payment = await client.CreateAsync(request);

```
```python
import mercadopago
sdk = mercadopago.SDK("ENV_ACCESS_TOKEN")

payment_data = {
    "transaction_amount": 100,
    "description": "Product Title",
    "payment_method_id": "rapipago",
    "payer": {
        "email": "test_user_84162205@testuser.com"
    }
}

payment_response = sdk.payment().create(payment_data)
payment = payment_response["response"]
```
```curl
curl -X POST \
  'https://api.mercadopago.com/v1/payments' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
  -d '{
  transaction_amount: 100,
  description: "Product Title",
  payment_method_id: "abitab",
  payer: { email: "test_user_84162205@testuser.com" }
}'
```
]]]

<br>
The response will show pending status until the buyer makes the payment. The payment voucher ID is the same as the Mercado Pago transaction ID.

```json
[
 {
    ...,
    "id": 5466310457,
    "status": "pending",
    "status_detail": "pending_waiting_payment",
    ...,
   "transaction_details": {
        "payment_method_reference_id": "24308188",
        "verification_code": "24308188",
        "net_received_amount": 0,
        "total_paid_amount": 100,
        "overpaid_amount": 0,
        "external_resource_url": "https://www.mercadopago.com/mlu/payments/sandbox/ticket/helper?payment_id=1234&payment_method_reference_id=12345678&caller_id=1234&hash=aaaaaa-bbb-cccc-dddd-eeeeeeee",
        "installment_amount": 0,
        "financial_institution": "",
        "payable_deferral_period": null,
        "acquirer_reference": null
   }
  }
]
```

In the `external_resource_url` field you will find an address with payment instructions for your buyer. You can redirect or send him/her the link.

> NOTE
>
> Note
>
> Customers have 3 to 5 days to pay, depending on the payment method. After that, you should cancel it.

## Cancel payments

To avoid collection issues, you need to cancel expired payments. Cash payments should be paid within 3 to 5 business days, based on their relevant term.

Take into account that **you can only cancel payments in process or pending status**. If a payment expires after 30 days, the cancellation is automatic and the final status will be cancelled or expired.

For more information, check the [Refunds and Cancellations section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/manage-account/account/cancellations-and-refunds).

## Payment credit times

Each payment method has its own credit times; it is immediate in some cases, while in others, it may take up to 3 business days.

Check [credit times by payment method](https://www.mercadopago[FAKER][URL][DOMAIN]/ayuda/Medios-de-pago-y-acreditaci-n_221) whenever you need to.

------------

----[mco]----

## How does it work?

To receive other payment methods, you should take into account two aspects:

1. First, you need a frontend to collect customer's e-mail and document, amount and payment method.
1. Then, you need a backend that takes the payment data and can confirm and make the payment.

Both for frontend and backend, we recommend [our libraries](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/previous-requirements/#bookmark_always_use_our_libraries) to collect user sensitive data securely.

## Check the available payment methods

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Payment Methods

In addition to cards, you can offer other payment choices on your website.

| Type of payment method | Payment method |
| --- | --- |
| `ticket` | Efecty |
| `ticket` | Baloto |
| `bank_transfer` | PSE |


### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Obtain the available payment methods

You can check the available payment methods whenever you need.

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment_methods = MercadoPago::get("/v1/payment_methods");

?>
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

payment_methods = mercadopago.get("/v1/payment_methods");
```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

payment_methods = MercadoPago.SDK.get("/v1/payment_methods");

```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_methods_response = sdk.payment_methods.get()
payment_methods = payment_methods_response[:response]

```
```csharp
using MercadoPago.Client.PaymentMethod;
using MercadoPago.Config;
using MercadoPago.Resource;
using MercadoPago.Resource.PaymentMethod;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var client = new PaymentMethodClient();
ResourcesList<PaymentMethod> paymentMethods = await client.ListAsync();

```
```python
import mercadopago
sdk = mercadopago.SDK("ACCESS_TOKEN")

payment_methods_response = sdk.payment_methods().list_all()
payment_methods = payment_methods_response["response"]
```
```curl
curl -X GET \
    -H 'accept: application/json' \
    -H 'content-type: application/json' \
    -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
    'https://api.mercadopago.com/v1/payment_methods' \
```
]]]

<br>

The result will be a list of payment methods and their features. For example, `payment_type_id` payment methods with `ticket` as value refer to cash payment method.

Keep in mind that the answer will return all the payments methods. For this reason, you have to filter the options you want to offer according to the [list of available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/other-payment-ways#bookmark_payment_methods).

```json
[
  {
        "id": "efecty",
        "name": "Efecty",
        "payment_type_id": "ticket",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/efecty.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/efecty.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": [],
        "min_allowed_amount": 5000,
        "max_allowed_amount": 4000000,
        "accreditation_time": 0,
        "financial_institutions": [],
        "processing_modes": [
            "aggregator"
        ]
    },
{
        "id": "baloto",
        "name": "Baloto",
        "payment_type_id": "ticket",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/baloto.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/baloto.gif",
        "deferred_capture": "supported",
        "settings": [],
        "additional_info_needed": [],
        "min_allowed_amount": 1500,
        "max_allowed_amount": 1000000,
        "accreditation_time": 0,
        "financial_institutions": [],
        "processing_modes": [
            "aggregator"
        ]
    },
{
        "id": "pse",
        "name": "PSE",
        "payment_type_id": "bank_transfer",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/pse.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/pse.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": [
            "entity_type"
        ],
        "min_allowed_amount": 1600,
        "max_allowed_amount": 30000000,
        "accreditation_time": 30,
        "financial_institutions": [
            …,
        ],
        "processing_modes": [
            "aggregator"
        ]
    }
]
```

> For more information, check [API References](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/reference).

<br>
<span></span>

> CLIENT_SIDE
>
> h2
>
> Data capture for payment

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Use MercadoPago.js library
**Remember to use our official library to access Mercado Pago API** from your application and collect data securely.

```html
<script src="https://secure.mlstatic.com/sdk/javascript/v1/mercadopago.js"></script>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Add payment form

To capture sensitive data from your customer, **please use our form with the corresponding attributes,** which ensures information security.

You can easily include anything you need and add your own style.

Use the list you consulted in [Obtain the available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-api/other-payment-ways#bookmark_obtain_the_available_payment_methods) to create the payment options you want to offer.

```html
<form action="/process_payment" method="post" id="paymentForm">
    <h3>Payment method</h3>
    <div>
      <select class="form-control" id="paymentMethod" name="paymentMethod">
        <option>Select a payment form</option>

        <!-- Create an option for each payment method with their name and complete the ID in the attribute 'value'. -->
        <option value="--PaymentTypeId--">--PaymentTypeName--</option>
      </select>
    </div>
    <h3>Buyer Details</h3>
    <div>
    <div>
        <label for="payerFirstName">Name</label>
        <input id="payerFirstName" name="payerFirstName" type="text" value="Nome"></select>
      </div>
      <div>
        <label for="payerLastName">Surname</label>
        <input id="payerLastName" name="payerLastName" type="text" value="Sobrenome"></select>
      </div>
      <div>
        <label for="payerEmail">E-mail</label>
        <input id="payerEmail" name="payerEmail" type="text" value="test@test.com"></select>
      </div>
      <div>
        <label for="docType">Document Type</label>
        <select id="docType" name="docType" data-checkout="docType" type="text"></select>
      </div>
      <div>
        <label for="docNumber">Document Number</label>
        <input id="docNumber" name="docNumber" data-checkout="docNumber" type="text"/>
      </div>
    </div>

    <div>
      <div>
        <input type="hidden" name="transactionAmount" id="transactionAmount" value="100" />
        <input type="hidden" name="productDescription" id="productDescription" value="Product name" />
        <br>
        <button type="submit">Pay</button>
        <br>
      </div>
  </div>
</form>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. Configure your public key

Add your [public key]([FAKER][CREDENTIALS][URL]) like this:

```javascript
window.Mercadopago.setPublishableKey("YOUR_PUBLIC_KEY");
```

>  If you still don't have an account to check your credentials, [sign in](https://www.mercadopago[FAKER][URL][DOMAIN]/registration-mp).

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4. Get data for your form

#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Get document types

Document type is one of the mandatory fields. Use the document list to fill out your data.

When you call the next function, MercadoPago.js will automatically fill out the available options, including the _select_ type element with `id = docType` in the form:

```javascript
window.Mercadopago.getIdentificationTypes();
```

> Find more information in the [Document type section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/identification-types).

<br>
<span></span>

> SERVER_SIDE
>
> h2
>
> Payment submission to Mercado Pago

To receive cash payments, just send your customer's e-mail and document, amount and payment method.

Once the request –with all the collected information– is in your backend, it should be submitted to Mercado Pago through our APIs.

For this to work, you should configure your [private key]([FAKER][CREDENTIALS][URL]).

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment = new MercadoPago\Payment();
  $payment->transaction_amount = 5000;
  $payment->description = "Product Title";
  $payment->payment_method_id = "efecty";
  $payment->payer = array(
    "email" => "test_user_19549678@testuser.com"
  );

  $payment->save();
?>
```
```node

var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

var payment_data = {
  transaction_amount: 5000,
  description: 'Product Title',
  payment_method_id: 'efecty',
  payer: {
    email: 'test_user_19549678@testuser.com'
  }
};

mercadopago.payment.create(payment_data).then(function (data) {

}).catch(function (error) {

});

```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

Payment payment = new Payment();

payment.setTransactionAmount(5000f)
      .setDescription("Product Title")
      .setPaymentMethodId("efecty")
      .setPayer(new Payer("test_user_19549678@testuser.com"));

payment.save();
```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_request = {
  transaction_amount: 5000,
  description: 'Product Title',
  payment_method_id: 'efecty',
  payer: {
    email: 'test_user_19549678@testuser.com',
  }
}

payment_response = sdk.payment.create(payment_request)
payment = payment_response[:response]
```
```csharp

using MercadoPago.Config;
using MercadoPago.Client.Payment;
using MercadoPago.Resource.Payment;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var request = new PaymentCreateRequest
{
    TransactionAmount = 5000,
    Description = "Product Title",
    PaymentMethodId = "efecty",
    Payer = new PaymentPayerRequest
    {
        Email = "test_user_19549678@testuser.com",
    },
};

var client = new PaymentClient();
Payment payment = await client.CreateAsync(request);

```
```python
import mercadopago
sdk = mercadopago.SDK("ENV_ACCESS_TOKEN")

payment_data = {
    "transaction_amount": 5000,
    "description": "Product Title",
    "payment_method_id": "efecty",
    "payer": {
        "email": "test_user_19549678@testuser.com"
    }
}

payment_response = sdk.payment().create(payment_data)
payment = payment_response["response"]
```
```curl
curl -X POST \
  'https://api.mercadopago.com/v1/payments' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
  -d '{
  transaction_amount: 5000,
  description: "Product Title",
  payment_method_id: "efecty",
  payer: { email: "test_user_19549678@testuser.com" }
}'
```
]]]

<br>
The response will show pending status until the buyer makes the payment. The payment voucher ID is the same as the Mercado Pago transaction ID.

```json
[
 {
    ...,
    "id": 5466310457,
    "status": "pending",
    "status_detail": "pending_waiting_payment",
    ...,
    "transaction_details": {
        "payment_method_reference_id": "24308386",
        "verification_code": "24308386",
        "net_received_amount": 0,
        "total_paid_amount": 5000,
        "overpaid_amount": 0,
        "external_resource_url": "https://www.mercadopago.com/mco/payments/sandbox/ticket/helper?payment_id=1234&payment_method_reference_id=12345678&caller_id=1234&hash=aaaaaa-bbb-cccc-dddd-eeeeeeee",
        "installment_amount": 0,
        "financial_institution": "",
        "payable_deferral_period": null,
        "acquirer_reference": null
   }
  }
]
```

In the `external_resource_url` field you will find an address with payment instructions for your buyer. You can redirect or send him/her the link.

> NOTE
>
> Note
>
> Customers have 3 to 5 days to pay, depending on the payment method. After that, you should cancel it.


## Expiration date of cash payment

If you want, you can change the default due date of a cash payment by sending the `date_of_expiration` field in the payment creation request. The configured date must be between 1 and 30 days from the issue date.

[[[
```php
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

$payment->date_of_expiration = "2020-05-30T23:59:59.000-04:00";
```
```node
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

date_of_expiration: "2020-05-30T23:59:59.000-04:00",
```
```java
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

payment.setDateOfExpiration("2020-05-30T23:59:59.000-04:00")
```
```ruby
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

date_of_expiration: '2020-05-30T23:59:59.000-04:00',
```
```csharp
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

paymentCreateRequest.DateOfExpiration = DateTime.Parse("2020-05-30T23:59:59.000-04:00");
```
```python
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

"date_of_expiration": "2020-05-30T23:59:59.000-04:00"
```
```curl
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

"date_of_expiration": "2020-05-30T23:59:59.000-04:00",
```
]]]

The deadline for approval of the cash payment is between 1 and 2 working days according to the payment method. Therefore, we recommend that you set the due date with at least 3 days to ensure that payment is made.

Check [credit times by payment method](https://www.mercadopago[FAKER][URL][DOMAIN]/ayuda/Medios-de-pago-y-acreditaci-n_221) when configuring.

> WARNING
>
> Important
>
> If the cash payment is paid after the expiration date, the amount will be refunded to the payer's Mercado Pago account.


## Receive PSE payments

To receive payments with PSE, you also have to send the financial institution that processes the payment.

[[[
```php
<?php

MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

$payment = new MercadoPago\Payment();
$payment->transaction_amount = 5000;
$payment->description = "Product Title";
$payment->payer = array (
"email" => "test_user_19549678@testuser.com",
"identification" => array(
"type" => "CC",
"number" => "76262349"
),
"entity_type" => "individual"
);
$payment->transaction_details = array(
"financial_institution" => 1234
);
$payment->additional_info = array(
"ip_address" => "127.0.0.1"
);
$payment->callback_url = "http://www.your-site.com";
$payment->payment_method_id = "pse";

$payment->save();

?>
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(ENV_ACCESS_TOKEN);

var payment_data = {
transaction_amount: 5000,
description: 'Product Title',
payer: {
email: 'test_user_19549678@testuser.com',
identification: {
type: "CC",
number: "76262349"
},
entity_type: "individual"
},
transaction_details: {
financial_institution: 1234
},
additional_info: {
ip_address: "127.0.0.1"
},
callback_url: "http://www.your-site.com",
payment_method_id: "pse"
}

mercadopago.payment.create(payment_data).then(function (data) {

}).catch(function (error) {

});
```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

Payer payer = new Payer();
payer.setEmail("test_user_19549678@testuser.com");
payer.setIdentification(new Identification("CC", 76262349));
payer.setEntityType("individual");

TransactionDetails transactionDetails = new TransactionDetails();
transactionDetails.financialInstitution = 1234;

AdditionalInfo additionalInfo = new AdditionalInfo();
additionalInfo.ipAddress = "127.0.0.1";

Payment payment = new Payment();
payment.setTransactionAmount(5000f)
.setDescription("Product Title")
.setPayer(payer)
.setTransactionDetails(transactionDetails)
.additionalInfo(additionalInfo)
.callbackUrl("http://www.your-site.com")
.setPaymentMethodId("pse");
```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_request = {
  transaction_amount: 5000,
  description: 'Product Title',
  additional_info: {
    ip_address: '127.0.0.1'
  },
  payer: {
    email: 'test_user_19549678@testuser.com',
    entity_type: 'individual'
  },
  transaction_details: {
    financial_institution: 1234
  },
  callback_url: 'http://www.your-site.com',
  payment_method_id: 'pse'
}

payment_response = sdk.payment.create(payment_request)
payment = payment_response[:response]
```
```python
import mercadopago
sdk = mercadopago.SDK("ENV_ACCESS_TOKEN")

payment_data = {
    "transaction_amount": 5000
    "description": 'Product Title'
    "payer": {
        "email": "test_user_19549678@testuser.com",
        "identification": {
            "type": "CC",
            "number": "76262349"
        },
        "entity_type": "individual"
    },
    "transaction_details": {
        "financial_institution": 1234
    },
    "additional_info": {
        "ip_address": "127.0.0.1"
    },
    "callback_url": "http://www.your-site.com"
    "payment_method_id": "pse"
}

payment_response = sdk.payment().create(payment_data)
payment = payment_response["response"]
```
```curl
curl -X POST \
'https://api.mercadopago.com/v1/payments' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer ACCESS_TOKEN' \
  -d '{
  transaction_amount: 5000,
  description: "Product Title",
  payment_method_id: "pse",
  payer: { email: "test_user_19549678@testuser.com" },
  transaction_details: { financial_institution: 1234 }
}'
```
]]]

The response will show pending status until the buyer makes the payment.

```json
[
 {
    ...,
	"status": "pending",
	"status_detail": "pending_waiting_transfer"
    ...,
	"transaction_details": {
		...,
		"external_resource_url": "https://www.mercadopago.com/mco/payments/bank_transfer/sandbox/helper/commerce?id=3692089&caller_id=1234&hash=aaaaaa-bbb-cccc-dddd-eeeeeeee",
		"installment_amount": 0,
		"financial_institution": "1234",
		"payment_method_reference_id": null
	}
 }
]
```
In the `external_resource_url` field you will find an address with payment instructions for your buyer. You can redirect or send him/her the link.

At the end of the payment, the client will be redirected to the `callback_url` you specify.

## Cancel payments

To avoid collection issues, you need to cancel expired payments. Cash payments should be paid within 3 to 5 business days, based on their relevant term.

Take into account that **you can only cancel payments in process or pending status**. If a payment expires after 30 days, the cancellation is automatic and the final status will be cancelled or expired.

For more information, check the [Refunds and Cancellations section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/manage-account/account/cancellations-and-refunds).

## Payment credit times

Each payment method has its own credit times; it is immediate in some cases, while in others, it may take up to 3 business days.

Check [credit times by payment method](https://www.mercadopago[FAKER][URL][DOMAIN]/ayuda/Medios-de-pago-y-acreditaci-n_221) whenever you need to.

------------

----[mlc]----

## How does it work?

To receive other payment methods, you should take into account two aspects:

1. First, you need a frontend to collect customer's e-mail and document, amount and payment method.
1. Then, you need a backend that takes the payment data and can confirm and make the payment.

Both for frontend and backend, we recommend [our libraries](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/previous-requirements/#bookmark_always_use_our_libraries) to collect user sensitive data securely.

## Check the available payment methods

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Payment Methods

In addition to cards, you can offer other payment choices on your website.

| Type of payment method | Payment method |
| --- | --- |
| `ticket` | Sucursales Servipag |
| `bank_transfer` | Redcompra Webpay |


### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Obtain the available payment methods

You can check the available payment methods whenever you need.

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment_methods = MercadoPago::get("/v1/payment_methods");

?>
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

payment_methods = mercadopago.get("/v1/payment_methods");
```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

payment_methods = MercadoPago.SDK.get("/v1/payment_methods");

```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_methods_response = sdk.payment_methods.get()
payment_methods = payment_methods_response[:response]

```
```csharp
using MercadoPago.Client.PaymentMethod;
using MercadoPago.Config;
using MercadoPago.Resource;
using MercadoPago.Resource.PaymentMethod;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var client = new PaymentMethodClient();
ResourcesList<PaymentMethod> paymentMethods = await client.ListAsync();

```
```python
import mercadopago
sdk = mercadopago.SDK("ACCESS_TOKEN")

payment_methods_response = sdk.payment_methods().list_all()
payment_methods = payment_methods_response["response"]
```
```curl
curl -X GET \
    -H 'accept: application/json' \
    -H 'content-type: application/json' \
    -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
    'https://api.mercadopago.com/v1/payment_methods' \
```
]]]

<br>

The result will be a list of payment methods and their features. For example, `payment_type_id` payment methods with `ticket` as value refer to cash payment method.

Keep in mind that the answer will return all the payments methods. For this reason, you have to filter the options you want to offer according to the [list of available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/other-payment-ways#bookmark_payment_methods).

```json
[
 {
        "id": "servipag",
        "name": "Sucursales Servipag",
        "payment_type_id": "ticket",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/servipag.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/servipag.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": [],
        "min_allowed_amount": 1,
        "max_allowed_amount": 500000,
        "accreditation_time": 0,
        "financial_institutions": [],
        "processing_modes": [
            "aggregator"
         ]
    },
 {
        "id": "webpay",
        "name": "Redcompra Webpay",
        "payment_type_id": "bank_transfer",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/webpay.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/webpay.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": [
            "entity_type"
        ],
        "min_allowed_amount": 50,
        "max_allowed_amount": 3000000,
        "accreditation_time": 20,
        "financial_institutions": [
            {
                "id": "1234",
                "description": "Transbank"
            }
        ],
        "processing_modes": [
            "aggregator"
        ]
    }
]
```

> For more information, check [API References](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/reference).

<br>
<span></span>

> CLIENT_SIDE
>
> h2
>
> Data capture for payment

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Use MercadoPago.js library
**Remember to use our official library to access Mercado Pago API** from your application and collect data securely.

```html
<script src="https://secure.mlstatic.com/sdk/javascript/v1/mercadopago.js"></script>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Add payment form

To capture sensitive data from your customer, **please use our form with the corresponding attributes,** which ensures information security.

You can easily include anything you need and add your own style.

Use the list you consulted in [Obtain the available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-api/other-payment-ways#bookmark_obtain_the_available_payment_methods) to create the payment options you want to offer.

```html
<form action="/process_payment" method="post" id="paymentForm">
    <h3>Payment method</h3>
    <div>
      <select class="form-control" id="paymentMethod" name="paymentMethod">
        <option>Select a payment form</option>

        <!-- Create an option for each payment method with their name and complete the ID in the attribute 'value'. -->
        <option value="--PaymentTypeId--">--PaymentTypeName--</option>
      </select>
    </div>
    <h3>Buyer Details</h3>
    <div>
    <div>
        <label for="payerFirstName">Name</label>
        <input id="payerFirstName" name="payerFirstName" type="text" value="Nome"></select>
      </div>
      <div>
        <label for="payerLastName">Surname</label>
        <input id="payerLastName" name="payerLastName" type="text" value="Sobrenome"></select>
      </div>
      <div>
        <label for="payerEmail">E-mail</label>
        <input id="payerEmail" name="payerEmail" type="text" value="test@test.com"></select>
      </div>
      <div>
        <label for="docType">Document Type</label>
        <select id="docType" name="docType" data-checkout="docType" type="text"></select>
      </div>
      <div>
        <label for="docNumber">Document Number</label>
        <input id="docNumber" name="docNumber" data-checkout="docNumber" type="text"/>
      </div>
    </div>

    <div>
      <div>
        <input type="hidden" name="transactionAmount" id="transactionAmount" value="100" />
        <input type="hidden" name="productDescription" id="productDescription" value="Product name" />
        <br>
        <button type="submit">Pay</button>
        <br>
      </div>
  </div>
</form>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. Configure your public key

Add your [public key]([FAKER][CREDENTIALS][URL]) like this:

```javascript
window.Mercadopago.setPublishableKey("YOUR_PUBLIC_KEY");
```

>  If you still don't have an account to check your credentials, [sign in](https://www.mercadopago[FAKER][URL][DOMAIN]/registration-mp).

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4. Get data for your form

#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Get document types

Document type is one of the mandatory fields. Use the document list to fill out your data.

When you call the next function, MercadoPago.js will automatically fill out the available options, including the _select_ type element with `id = docType` in the form:

```javascript
window.Mercadopago.getIdentificationTypes();
```

> Find more information in the [Document type section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/identification-types).

<br>
<span></span>

> SERVER_SIDE
>
> h2
>
> Payment submission to Mercado Pago

To receive cash payments, just send your customer's e-mail and document, amount and payment method.

Once the request –with all the collected information– is in your backend, it should be submitted to Mercado Pago through our APIs.

For this to work, you should configure your [private key]([FAKER][CREDENTIALS][URL]).

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment = new MercadoPago\Payment();
  $payment->transaction_amount = 100;
  $payment->description = "Product Title";
  $payment->payment_method_id = "servipag";
  $payment->payer = array(
    "email" => "test_user_15748052@testuser.com"
  );

  $payment->save();
?>
```
```node

var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

var payment_data = {
  transaction_amount: 100,
  description: 'Product Title',
  payment_method_id: 'servipag',
  payer: {
    email: 'test_user_15748052@testuser.com'
  }
};

mercadopago.payment.create(payment_data).then(function (data) {

}).catch(function (error) {

});

```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

Payment payment = new Payment();

payment.setTransactionAmount(100f)
      .setDescription("Product Title")
      .setPaymentMethodId("servipag")
      .setPayer(new Payer("test_user_15748052@testuser.com"));

payment.save();
```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_request = {
  description: 'Product Title',
  transaction_amount: 100,
  payment_method_id: 'servipag',
  payer: {
    email: 'test_user_15748052@testuser.com'
  }
}

payment_response = sdk.payment.create(payment_request)
payment = payment_response[:response]
```
```csharp

using MercadoPago.Config;
using MercadoPago.Client.Payment;
using MercadoPago.Resource.Payment;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var request = new PaymentCreateRequest
{
    TransactionAmount = 5000,
    Description = "Product Title",
    PaymentMethodId = "servipag",
    Payer = new PaymentPayerRequest
    {
        Email = "test_user_15748052@testuser.com",
    },
};

var client = new PaymentClient();
Payment payment = await client.CreateAsync(request);

```
```python
import mercadopago
sdk = mercadopago.SDK("ENV_ACCESS_TOKEN")

payment_data = {
    "transaction_amount": 100,
    "description": "Product Title",
    "payment_method_id": "servipag",
    "payer": {
        "email": "test_user_15748052@testuser.com"
    }
}

payment_response = sdk.payment().create(payment_data)
payment = payment_response["response"]
```
```curl
curl -X POST \
  'https://api.mercadopago.com/v1/payments' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
  -d '{
  transaction_amount: 100,
  description: "Product Title",
  payment_method_id: "servipag",
  payer: { email: "test_user_15748052@testuser.com" }
}'
```
]]]

<br>
The response will show pending status until the buyer makes the payment. The payment voucher ID is the same as the Mercado Pago transaction ID.

```json
[
 {
    ...,
    "id": 5466310457,
    "status": "pending",
    "status_detail": "pending_waiting_payment",
    ...,
   "transaction_details": {
        "payment_method_reference_id": "24308616",
        "verification_code": "24308615",
        "net_received_amount": 0,
        "total_paid_amount": 100,
        "overpaid_amount": 0,
        "external_resource_url": "https://www.mercadopago.com/mlc/payments/sandbox/ticket/helper?payment_id=1234&payment_method_reference_id=12345678&caller_id=1234&hash=aaaaaa-bbb-cccc-dddd-eeeeeeee",
        "installment_amount": 0,
        "financial_institution": "",
        "payable_deferral_period": null,
        "acquirer_reference": null
   }
  }
]
```

In the `external_resource_url` field you will find an address with payment instructions for your buyer. You can redirect or send him/her the link.

> NOTE
>
> Note
>
> Customers have 3 to 5 days to pay, depending on the payment method. After that, you should cancel it.

## Receive Webpay payments

To receive payments with Webpay, you have to send the IP address of the buyer, the financial institution that processes the payment and, optionally, the RUT and the type of person.

[[[
```php
<?php

MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

$payment = new MercadoPago\Payment();
$payment->transaction_amount = 100;
$payment->description = "Product Title";
$payment->payer = array (
"email" => "test_user_15748052@testuser.com",
"entity_type" => "individual"
);
$payment->transaction_details = array(
"financial_institution" => 1234
);
$payment->additional_info = array(
"ip_address" => "127.0.0.1"
);
$payment->callback_url = "http://www.your-site.com";
$payment->payment_method_id = "webpay";

$payment->save();

?>
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(ENV_ACCESS_TOKEN);

var payment_data = {
transaction_amount: 100,
description: 'Product Title',
payer: {
email: 'test_user_15748052@testuser.com',
entity_type: "individual"
},
transaction_details: {
financial_institution: 1234
},
additional_info: {
ip_address: "127.0.0.1"
},
callback_url: "http://www.your-site.com",
payment_method_id: "webpay"
}

mercadopago.payment.create(payment_data).then(function (data) {

}).catch(function (error) {

});
```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

Payer payer = new Payer();
payer.setEmail("test_user_15748052@testuser.com");
payer.setEntityType("individual");

TransactionDetails transactionDetails = new TransactionDetails();
transactionDetails.financialInstitution = 1234;

AdditionalInfo additionalInfo = new AdditionalInfo();
additionalInfo.ipAddress = "127.0.0.1";

Payment payment = new Payment();
payment.setTransactionAmount(100f)
.setDescription("Product Title")
.setPayer(payer)
.setTransactionDetails(transactionDetails)
.additionalInfo(additionalInfo)
.callbackUrl("http://www.your-site.com")
.setPaymentMethodId("webpay");

payment.save();
```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_request = {
  transaction_amount: 100,
  description: 'Product Title',
  payer: {
    email: 'test_user_15748052@testuser.com',
    entity_type: 'individual'
  },
  transaction_details: {
    financial_institution: 1234
  },
  additional_info: {
    ip_address: '127.0.0.1'
  },
  callback_url: 'http://www.your-site.com',
  payment_method_id: 'webpay'
}

payment_response = sdk.payment.create(payment_request)
payment = payment_response[:response]
```
```python
import mercadopago
sdk = mercadopago.SDK("ENV_ACCESS_TOKEN")

payment_data = {
    "transaction_amount": 100
    "description": 'Product Title'
    "payer": {
        "email": "test_user_15748052@testuser.com",
        "entity_type": "individual
    },
    "transaction_details": {
        "financial_institution": 1234
    },
    "additional_info": {
        "ip_address": "127.0.0.1"
    },
    "callback_url": "http://www.your-site.com"
    "payment_method_id": "webpay"
}

payment_response = sdk.payment().create(payment_data)
payment = payment_response["response"]
```
```curl
curl -X POST \
'https://api.mercadopago.com/v1/payments' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer ACCESS_TOKEN' \
  -d '{
  transaction_amount: 100,
  description: "Product Title",
  payment_method_id: "webpay",
  payer: {
    email: "test_user_15748052@testuser.com",
    identification: {
    type: "RUT",
    number: 76262349
},
  entity_type: "individual"
},
  transaction_details: {
    financial_institution: 1234
},
  additional_info: {
  ip_address: “127.0.0.1”
},
  callback_url: "http://www.your-site.com"
}'
```
]]]

> The expected `entity_type` are individual (individuals) or association (companies).

The response will show pending status until the buyer makes the payment.

```json
[
 {
    ...,
	"status": "pending",
	"status_detail": "pending_waiting_transfer"
    ...,
	"transaction_details": {
		...,
		"external_resource_url": "https://www.mercadopago.com/mlc/payments/bank_transfer/sandbox/helper/commerce?id=3692089&caller_id=1234&hash=aaaaaa-bbb-cccc-dddd-eeeeeeee",
		"installment_amount": 0,
		"financial_institution": "1234",
		"payment_method_reference_id": null
	}
 }
]
```

In the `external_resource_url` field you will find an address with payment instructions for your buyer. You can redirect or send him/her the link.

At the end of the payment, the client will be redirected to the `callback_url` that you specify with the following parameters `payment_id=6725591786&payment_status=approved&external_reference=null&payment_method_id=webpay`.

## Cancel payments

To avoid collection issues, you need to cancel expired payments. Cash payments should be paid within 3 to 5 business days, based on their relevant term.

Take into account that **you can only cancel payments in process or pending status**. If a payment expires after 30 days, the cancellation is automatic and the final status will be cancelled or expired.

> WARNING
>
> Important
>
> Remember that if payments are not made within 30 minutes, Webpay will automatically cancel them.

For more information, check the [Refunds and Cancellations section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/manage-account/account/cancellations-and-refunds).

## Payment credit times

Each payment method has its own credit times; it is immediate in some cases, while in others, it may take up to 3 business days.

Check [credit times by payment method](https://www.mercadopago.cl/ayuda/Medios-de-pago-y-acreditaci-n_221) whenever you need to.

------------


----[mpe]----

## How does it work?

To receive other payment methods, you should take into account two aspects:

1. First, you need a frontend to collect customer's e-mail and document, amount and payment method.
1. Then, you need a backend that takes the payment data and can confirm and make the payment.

Both for frontend and backend, we recommend [our libraries](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/previous-requirements/#bookmark_always_use_our_libraries) to collect user sensitive data securely.

## Check the available payment methods

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Payment Methods

In addition to cards, you can offer other payment choices on your website.

| Type of payment method | Payment method |
| --- | --- |
| `atm` | PagoEfectivo |


### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Obtain the available payment methods

You can check the available payment methods whenever you need.

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment_methods = MercadoPago::get("/v1/payment_methods");

?>
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

payment_methods = mercadopago.get("/v1/payment_methods");
```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

payment_methods = MercadoPago.SDK.get("/v1/payment_methods");

```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_methods_response = sdk.payment_methods.get()
payment_methods = payment_methods_response[:response]

```
```csharp
using MercadoPago.Client.PaymentMethod;
using MercadoPago.Config;
using MercadoPago.Resource;
using MercadoPago.Resource.PaymentMethod;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var client = new PaymentMethodClient();
ResourcesList<PaymentMethod> paymentMethods = await client.ListAsync();

```
```python
import mercadopago
sdk = mercadopago.SDK("ACCESS_TOKEN")

payment_methods_response = sdk.payment_methods().list_all()
payment_methods = payment_methods_response["response"]
```
```curl
curl -X GET \
    -H 'accept: application/json' \
    -H 'content-type: application/json' \
    -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
    'https://api.mercadopago.com/v1/payment_methods' \
```
]]]

<br>

The result will be a list of payment methods and their features. For example, `payment_type_id` payment methods with `ticket` as value refer to cash payment method.

Keep in mind that the answer will return all the payments methods. For this reason, you have to filter the options you want to offer according to the [list of available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/other-payment-ways#bookmark_payment_methods).

```json
[
  {
      "id": "pagoefectivo_atm",
      "name": "BCP, BBVA Continental u otros",
      "payment_type_id": "atm",
      "status": "active",
      "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/pagoefectivo_atm.gif",
      "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/pagoefectivo_atm.gif",
      "deferred_capture": "does_not_apply",
      "settings": [],
      "additional_info_needed": [],
      "min_allowed_amount": 5,
      "max_allowed_amount": 10000,
      "accreditation_time": 2880,
      "financial_institutions": [],
      "processing_modes": [
          "aggregator"
      ]
  }
]
```

> For more information, check [API References](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/reference).

<br>
<span></span>

> CLIENT_SIDE
>
> h2
>
> Data capture for payment

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Use MercadoPago.js library
**Remember to use our official library to access Mercado Pago API** from your application and collect data securely.

```html
<script src="https://secure.mlstatic.com/sdk/javascript/v1/mercadopago.js"></script>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Add payment form

To capture sensitive data from your customer, **please use our form with the corresponding attributes,** which ensures information security.

You can easily include anything you need and add your own style.

Use the list you consulted in [Obtain the available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-api/other-payment-ways#bookmark_obtain_the_available_payment_methods) to create the payment options you want to offer.

```html
<form action="/process_payment" method="post" id="paymentForm">
    <h3>Payment method</h3>
    <div>
      <select class="form-control" id="paymentMethod" name="paymentMethod">
        <option>Select a payment form</option>

        <!-- Create an option for each payment method with their name and complete the ID in the attribute 'value'. -->
        <option value="--PaymentTypeId--">--PaymentTypeName--</option>
      </select>
    </div>
    <h3>Buyer Details</h3>
    <div>
    <div>
        <label for="payerFirstName">Name</label>
        <input id="payerFirstName" name="payerFirstName" type="text" value="Nome"></select>
      </div>
      <div>
        <label for="payerLastName">Surname</label>
        <input id="payerLastName" name="payerLastName" type="text" value="Sobrenome"></select>
      </div>
      <div>
        <label for="payerEmail">E-mail</label>
        <input id="payerEmail" name="payerEmail" type="text" value="test@test.com"></select>
      </div>
      <div>
        <label for="docType">Document Type</label>
        <select id="docType" name="docType" data-checkout="docType" type="text"></select>
      </div>
      <div>
        <label for="docNumber">Document Number</label>
        <input id="docNumber" name="docNumber" data-checkout="docNumber" type="text"/>
      </div>
    </div>

    <div>
      <div>
        <input type="hidden" name="transactionAmount" id="transactionAmount" value="100" />
        <input type="hidden" name="productDescription" id="productDescription" value="Product name" />
        <br>
        <button type="submit">Pay</button>
        <br>
      </div>
  </div>
</form>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. Configure your public key

Add your [public key]([FAKER][CREDENTIALS][URL]) like this:

```javascript
window.Mercadopago.setPublishableKey("YOUR_PUBLIC_KEY");
```

>  If you still don't have an account to check your credentials, [sign in](https://www.mercadopago[FAKER][URL][DOMAIN]/registration-mp).

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4. Get data for your form

#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Get document types

Document type is one of the mandatory fields. Use the document list to fill out your data.

When you call the next function, MercadoPago.js will automatically fill out the available options, including the _select_ type element with `id = docType` in the form:

```javascript
window.Mercadopago.getIdentificationTypes();
```

> Find more information in the [Document type section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/identification-types).

<br>
<span></span>

> SERVER_SIDE
>
> h2
>
> Payment submission to Mercado Pago

To receive cash payments, just send your customer's e-mail and document, amount and payment method.

Once the request –with all the collected information– is in your backend, it should be submitted to Mercado Pago through our APIs.

For this to work, you should configure your [private key]([FAKER][CREDENTIALS][URL]).

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment = new MercadoPago\Payment();
  $payment->transaction_amount = 100;
  $payment->description = "Product Title";
  $payment->payment_method_id = "pagoefectivo_atm";
  $payment->payer = array(
    "email" => "test_user_42972582@testuser.com"
  );

  $payment->save();
?>
```
```node

var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

var payment_data = {
  transaction_amount: 100,
  description: 'Product Title',
  payment_method_id: 'pagoefectivo_atm',
  payer: {
    email: 'test_user_42972582@testuser.com'
  }
};

mercadopago.payment.create(payment_data).then(function (data) {

}).catch(function (error) {

});

```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

Payment payment = new Payment();

payment.setTransactionAmount(100f)
      .setDescription("Product Title")
      .setPaymentMethodId("pagoefectivo_atm")
      .setPayer(new Payer("test_user_42972582@testuser.com"));

payment.save();
```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_request = {
  transaction_amount: 100,
  description: 'Product Title',
  payment_method_id: 'pagoefectivo_atm',
  payer: {
    email: 'test_user_42972582@testuser.com'
  }
}

payment_response = sdk.payment.create(payment_request)
payment = payment_response[:response]
```
```csharp

using MercadoPago.Config;
using MercadoPago.Client.Payment;
using MercadoPago.Resource.Payment;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var request = new PaymentCreateRequest
{
    TransactionAmount = 5000,
    Description = "Product Title",
    PaymentMethodId = "pagoefectivo_atm",
    Payer = new PaymentPayerRequest
    {
        Email = "test_user_42972582@testuser.com",
    },
};

var client = new PaymentClient();
Payment payment = await client.CreateAsync(request);

```
```python
import mercadopago
sdk = mercadopago.SDK("ENV_ACCESS_TOKEN")

payment_data = {
    "transaction_amount": 100,
    "description": "Product Title",
    "payment_method_id": "pagoefectivo_atm",
    "payer": {
        "email": "test_user_42972582@testuser.com"
    }
}

payment_response = sdk.payment().create(payment_data)
payment = payment_response["response"]
```
```curl
curl -X POST \
  'https://api.mercadopago.com/v1/payments' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
  -d '{
  transaction_amount: 100,
  description: "Product Title",
  payment_method_id: "pagoefectivo_atm",
  payer: { email: "test_user_42972582@testuser.com" }
}'
```
]]]

<br>
The response will show pending status until the buyer makes the payment. The payment voucher ID is the same as the Mercado Pago transaction ID.

```json
[
  {
    ...,
    "id": 5466310457,
    "status": "pending",
    "status_detail": "pending_waiting_payment",
    ...,

    "order": {},
    "external_reference": null,
    "transaction_amount": 100,
    "transaction_amount_refunded": 0,
    "coupon_amount": 0,
    "differential_pricing_id": null,
    "deduction_schema": null,
    "transaction_details": {
        "payment_method_reference_id": "123457986",
        "verification_code": "24308767",
        "net_received_amount": 0,
        "total_paid_amount": 100,
        "overpaid_amount": 0,
        "external_resource_url": "https://www.mercadopago.com/mpe/payments/sandbox/atm/helper?payment_id=24308766&payment_method_reference_id=24308767&caller_id=537490079&hash=c96a61b0-10f4-40f6-afff-82fc0f0923da",
        "installment_amount": 0,
        "financial_institution": "PagoEfectivo",
        "payable_deferral_period": null,
        "acquirer_reference": null
    }
  }
]
```

In the `external_resource_url` field you will find an address with payment instructions for your buyer. You can redirect or send him/her the link.

> NOTE
>
> Note
>
> Customers have 3 to 5 days to pay, depending on the payment method. After that, you should cancel it.

## Cancel payments

To avoid collection issues, you need to cancel expired payments. Cash payments should be paid within 3 to 5 business days, based on their relevant term.

Take into account that **you can only cancel payments in process or pending status**. If a payment expires after 30 days, the cancellation is automatic and the final status will be cancelled or expired.

For more information, check the [Refunds and Cancellations section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/manage-account/account/cancellations-and-refunds).

## Payment credit times

Each payment method has its own credit times; it is immediate in some cases, while in others, it may take up to 3 business days.

Check [credit times by payment method](https://www.mercadopago[FAKER][URL][DOMAIN]/ayuda/Medios-de-pago-para-tus-compradores_2433) whenever you need to.

------------

----[mlb]----

## How does it work?

To receive other payment methods, you should take into account two aspects:

1. First, you need a frontend to collect data required for payment.

1. Then, you need a backend that takes the payment data and can confirm and make the payment.

Both for frontend and backend, we recommend [our libraries](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/previous-requirements/#bookmark_always_use_our_libraries) to collect user sensitive data securely.

## Check the available payment methods

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Payment Methods

In addition to cards, you can offer other payment choices on your website.

| Type of payment method | Payment method |
| --- | --- |
| `ticket` | Boleto |
| `ticket` | Pagamento em lotérica |
| `bank_transfer` | Pix |

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Obtain the available payment methods

You can check the available payment methods whenever you need.

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment_methods = MercadoPago::get("/v1/payment_methods");

?>
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

payment_methods = mercadopago.get("/v1/payment_methods");
```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

payment_methods = MercadoPago.SDK.get("/v1/payment_methods");

```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_methods_response = sdk.payment_methods.get()
payment_methods = payment_methods_response[:response]

```
```csharp
using MercadoPago.Client.PaymentMethod;
using MercadoPago.Config;
using MercadoPago.Resource;
using MercadoPago.Resource.PaymentMethod;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var client = new PaymentMethodClient();
ResourcesList<PaymentMethod> paymentMethods = await client.ListAsync();

```
```python
import mercadopago
sdk = mercadopago.SDK("ACCESS_TOKEN")

payment_methods_response = sdk.payment_methods().list_all()
payment_methods = payment_methods_response["response"]
```
```curl
curl -X GET \
    -H 'accept: application/json' \
    -H 'content-type: application/json' \
    -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
    'https://api.mercadopago.com/v1/payment_methods'
```
]]]

<br>

The result will be a list of payment methods and their features. For example, `payment_type_id` payment methods with `ticket` as value refer to cash payment method and `bank_transfer` refer to bank transfers.

Keep in mind that the answer will return all the payments methods. For this reason, you have to filter the options you want to offer according to the [list of available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/other-payment-ways#bookmark_payment_methods).

```json
[
    {
        "id": "bolbradesco",
        "name": "Boleto",
        "payment_type_id": "ticket",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/bolbradesco.gif",
        "thumbnail": "http://img.mlstatic.com/org-img/MP3/API/logos/bolbradesco.gif",
        "deferred_capture": "does_not_apply",
        "settings": [],
        "additional_info_needed": []
    },
    {
        "id": "pec",
        "name": "Pagamento na lotérica sem boleto",
        "payment_type_id": "ticket",
        "status": "active",
        "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/pec.gif",
        "thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/pec.gif",
        "deferred_capture": "supported",
        "settings": [],
        "additional_info_needed": []
    },
    {
       "id": "pix",
       "name": "PIX",
       "payment_type_id": "bank_transfer",
       "status": "active",
       "secure_thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/pix.gif",
       "thumbnail": "https://www.mercadopago.com/org-img/MP3/API/logos/pix.gif",
       "deferred_capture": "does_not_apply",
       "settings": [],
       "additional_info_needed": []
    },
    {
        "...": "..."
    }
]
```

> For more information, check [API References](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/reference).

<br>

> CLIENT_SIDE
>
> h2
>
> Data capture for payment

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Use MercadoPago.js library

**Remember to use our official library to access Mercado Pago API** from your application and collect data securely.

```html
<script src="https://secure.mlstatic.com/sdk/javascript/v1/mercadopago.js"></script>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Add payment form

To capture sensitive data from your customer, **please use our form with the corresponding attributes,** which ensures information security.

You can easily include anything you need and add your own style.

Use the list you consulted in [Obtain the available payment methods](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-api/other-payment-ways#bookmark_obtain_the_available_payment_methods) to create the payment options you want to offer.

```html
<form action="/process_payment" method="post" id="paymentForm">
    <h3>Payment method</h3>
    <div>
      <select class="form-control" id="paymentMethod" name="paymentMethod">
        <option>Select a payment form</option>

        <!-- Create an option for each payment method with their name and complete the ID in the attribute 'value'. -->
        <option value="--PaymentTypeId--">--PaymentTypeName--</option>
      </select>
    </div>
    <h3>Buyer Details</h3>
    <div>
    <div>
        <label for="payerFirstName">Name</label>
        <input id="payerFirstName" name="payerFirstName" type="text" value="Nome"></select>
      </div>
      <div>
        <label for="payerLastName">Surname</label>
        <input id="payerLastName" name="payerLastName" type="text" value="Sobrenome"></select>
      </div>
      <div>
        <label for="payerEmail">E-mail</label>
        <input id="payerEmail" name="payerEmail" type="text" value="test@test.com"></select>
      </div>
      <div>
        <label for="docType">Document Type</label>
        <select id="docType" name="docType" data-checkout="docType" type="text"></select>
      </div>
      <div>
        <label for="docNumber">Document Number</label>
        <input id="docNumber" name="docNumber" data-checkout="docNumber" type="text"/>
      </div>
    </div>

    <div>
      <div>
        <input type="hidden" name="transactionAmount" id="transactionAmount" value="100" />
        <input type="hidden" name="productDescription" id="productDescription" value="Product name" />
        <br>
        <button type="submit">Pay</button>
        <br>
      </div>
  </div>
</form>
```

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. Configure your public key

Add your [public key]([FAKER][CREDENTIALS][URL]) like this:

```javascript
window.Mercadopago.setPublishableKey("YOUR_PUBLIC_KEY");
```

>  If you still don't have an account to check your credentials, [sign in](https://www.mercadopago[FAKER][URL][DOMAIN]/registration-mp).

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4. Get data for your form

#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Get document types

Document type is one of the mandatory fields. Use the document list to fill out your data.

When you call the next function, MercadoPago.js will automatically fill out the available options, including the _select_ type element with `id = docType` in the form:

```javascript
window.Mercadopago.getIdentificationTypes();
```

> Find more information in the [Document type section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/identification-types).

<br>
<span></span>

> SERVER_SIDE
>
> h2
>
> Receive payments with Pix

You can receive payment immediately with Pix from any bank or digital wallet using QR code or payment code.

### Prerequisite

#### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Get a Pix key

To start, you will need a Pix key registered in the seller's account. This is unique data for account identification to use the payment method functionality.

[Learn how to create a Pix key](https://www.mercadopago.com.br/ajuda/17843)

> Note that the Pix key registration may take a few minutes.

### Payment data

After [capturing the data for payment](#bookmark_data_capture_for_payment) in the form, to start getting Pix payments you will need to dend the buyer's email address, document type and number, method of payment and amount.

[[[
```php
<?php

 require_once 'vendor/autoload.php';

 MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

 $payment = new MercadoPago\Payment();sssss
 $payment->transaction_amount = 100;
 $payment->description = "Product Title";
 $payment->payment_method_id = "pix";
 $payment->payer = array(
     "email" => "test@test.com",
     "first_name" => "Test",
     "last_name" => "User",
     "identification" => array(
         "type" => "CPF",
         "number" => "19119119100"
      ),
     "address"=>  array(
         "zip_code" => "06233200",
         "street_name" => "Av. das Nações Unidas",
         "street_number" => "3003",
         "neighborhood" => "Bonfim",
         "city" => "Osasco",
         "federal_unit" => "SP"
      )
   );

 $payment->save();

?>
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

var payment_data = {
  transaction_amount: 100,
  description: 'Product Title',
  payment_method_id: 'pix',
  payer: {
    email: 'test@test.com',
    first_name: 'Test',
    last_name: 'User',
    identification: {
        type: 'CPF',
        number: '19119119100'
    },
    address:  {
        zip_code: '06233200',
        street_name: 'Av. das Nações Unidas',
        street_number: '3003',
        neighborhood: 'Bonfim',
        city: 'Osasco',
        federal_unit: 'SP'
    }
  }
};

mercadopago.payment.create(payment_data).then(function (data) {

}).catch(function (error) {

});

```
```java
import com.mercadopago.*;

MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

Payment payment = new Payment();

payment.setTransactionAmount(100f)
       .setDescription("Product Title")
       .setPaymentMethodId("pix")
       .setPayer(new Payer()
           .setEmail("test@test.com")
           .setFirstName("Test")
           .setLastName("User")
           .setIdentification(new Identification()
               .setType("CPF")
               .setNumber("19119119100"))
           .setAddress(new Address()
               .setZipCode("06233200")
               .setStreetName("Av. das Nações Unidas")
               .setStreetNumber(3003)
               .setNeighborhood("Bonfim")
               .setCity("Osasco")
               .setFederalUnit("SP"))
);

payment.save();
```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_request = {
  transaction_amount: 100,
  description: "Product Title",
  payment_method_id: "pix",
  payer: {
    email: 'test@test.com',
    first_name: 'Test',
    last_name: 'User',
    identification: {
      type: 'CPF',
      number: '19119119100',
    },
    address: {
      zip_code: '06233200',
      street_name: 'Av. das Nações Unidas',
      street_number: '3003',
      neighborhood: 'Bonfim',
      city: 'Osasco',
      federal_unit: 'SP'
    }
  }
}

payment_response = sdk.payment.create(payment_request)
payment = payment_response[:response]

```
```csharp

using MercadoPago.Config;
using MercadoPago.Client.Common;
using MercadoPago.Client.Payment;
using MercadoPago.Resource.Payment;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var request = new PaymentCreateRequest
{
    TransactionAmount = 105,
    Description = "Product Title",
    PaymentMethodId = "pix",
    Payer = new Payer(){
        Email = "test@test.com",
        FirstName = "Test",
        LastName = "User",
        Identification = new IdentificationRequest
        {
            Type = "CPF",
            Number = "191191191-00",
        },
    },
};

var client = new PaymentClient();
Payment payment = await client.CreateAsync(request);

```
```python
import mercadopago
sdk = mercadopago.SDK("ENV_ACCESS_TOKEN")

payment_data = {
    "transaction_amount": 100,
    "description": "Product Title",
    "payment_method_id": "bolbradesco",
    "payer": {
        "email": "test@test.com",
        "first_name": "Test",
        "last_name": "User",
        "identification": {
            "type": "CPF",
            "number": "191191191-00"
        },
        "address": {
            "zip_code": "06233-200",
            "street_name": "Av. das Nações Unidas",
            "street_number": "3003",
            "neighborhood": "Bonfim",
            "city": "Osasco",
            "federal_unit": "SP"
        }
    }
}

payment_response = sdk.payment().create(payment_data)
payment = payment_response["response"]
```
```curl
curl -X POST \
    -H 'accept: application/json' \
    -H 'content-type: application/json' \
    -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
    'https://api.mercadopago.com/v1/payments' \
    -d '{
      "transaction_amount": 100,
      "description": "Product Title",
      "payment_method_id": "pix",
      "payer": {
        "email": "test@test.com",
        "first_name": "Test",
        "last_name": "User",
        "identification": {
            "type": "CPF",
            "number": "19119119100"
        },
        "address": {
            "zip_code": "06233200",
            "street_name": "Av. das Nações Unidas",
            "street_number": "3003",
            "neighborhood": "Bonfim",
            "city": "Osasco",
            "federal_unit": "SP"
        }
      }
    }'
```
]]]

<br>

The response will show the pending payment status and all the information needed to show to the buyer.

> WARNING
>
> Important
>
> Remember that in order to generate payments, you must first [create a Pix key](https://www.mercadopago.com.br/ajuda/17843).


The `transaction_data` will provide the data to make the payment available via QR code. These are the attributes: 

| Attributes| Description |
| --- | --- |
| `qr_code_base64` | Data for QR code render.|
| `qr_code` | Data to make payment code available for copy and paste. |


```json
[
 {
    ...,
    "id": 5466310457,
    "status": "pending",
    "status_detail": "pending_waiting_transfer",
    ...,
    "transaction_details": {
        "net_received_amount": 0,
        "total_paid_amount": 100,
        "overpaid_amount": 0,
        "external_resource_url": "https://www.mercadopago.com/mlb/payments/bank_transfer/helper?payment_id=123456789&payment_method_reference_id= 123456789&caller_id=123456",
        "installment_amount": 0,
        "financial_institution": null,
        "point_of_interaction": {
            "type": "PIX",
            "sub_type": null,
            "application_data": {
                "name": "NAME_SDK",
                "version": "VERSION_NUMBER",
            },
            "transaction_data": {
                "qr_code_base64": "iVBORw0KGgoAAAANSUhEUgAABRQAAAUUCAYAAACu5p7oAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAAgY0hSTQAAeiYAAICEAAD6AAAAgOgAAHUwAADqYAAAOpgAABdwnLpRPAAAIABJREFUeJzs2luO3LiWQNFmI+Y/Zd6vRt36KGNXi7ZOBtcagHD4kNLeiLX33v8DAAAAABD879sDAAAAAAA/h6AIAAAAAGSCIgAAAACQCYoAAAAAQCYoAgAAAACZoAgAAAAAZIIiAAAAAJAJigAAAABAJigCAAAAAJmgCAAAAABkgiIAAAAAkAmKAAAAAEAmKAIAAAAAmaAIAAAAAGSCIgAAAACQCYoAAAAAQCYoAgAAAACZoAgAAAAAZIIiAAAAAJAJigAAAABAJigCA...",
                "qr_code": "00020126600014br.gov.bcb.pix0117test@testuser.com0217additional data520400005303986540510.005802BR5913Maria Silva6008Brasilia62070503***6304E2CA",
            }
        }
    }
    ...,
  }
]∫
```

### Data to make payment

To make the payment, render the QR code to show it. You can also add an option to copy and paste payment code to make the transaction via Internet Banking.

Add `qr_code_base64` to show the QR code. For example, you can render it like this:

```html
<img src={`data:image/jpeg;base64,${qr_code_base64}`/>
```

To show the payment code for copy and paste, you can add `qr_code` like this:

```html
<label for="copy">Copy Hash:</label>
<input type="text" id="copy"  value={qr_code}/>
```

> NOTE
>
> To consider
>
>  * By default, the customer has 24 hours to make payment.
>  * The code can be used once only.
>  * The code will be shown provided that it is current per date of expiration.
<br>


> SERVER_SIDE
>
> h2
>
> Receive payments with boleto or lotérica

After [capturing the data for payment](#bookmark_data_capture_for_payment) with the form, to receive payments with boleto or lotérica, just send your customer's e-mail and document, amount and payment method.

Once the request –with all the collected information– is in your backend, it should be submitted to Mercado Pago through our APIs.

For this to work, you should configure your [private key]([FAKER][CREDENTIALS][URL]).

[[[
```php
<?php

 require_once 'vendor/autoload.php';

 MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

 $payment = new MercadoPago\Payment();
 $payment->transaction_amount = 100;
 $payment->description = "Product Title";
 $payment->payment_method_id = "bolbradesco";
 $payment->payer = array(
     "email" => "test@test.com",
     "first_name" => "Test",
     "last_name" => "User",
     "identification" => array(
         "type" => "CPF",
         "number" => "19119119100"
      ),
     "address"=>  array(
         "zip_code" => "06233200",
         "street_name" => "Av. das Nações Unidas",
         "street_number" => "3003",
         "neighborhood" => "Bonfim",
         "city" => "Osasco",
         "federal_unit" => "SP"
      )
   );

 $payment->save();

?>
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

var payment_data = {
  transaction_amount: 100,
  description: 'Product Title',
  payment_method_id: 'bolbradesco',
  payer: {
    email: 'test@test.com',
    first_name: 'Test',
    last_name: 'User',
    identification: {
        type: 'CPF',
        number: '19119119100'
    },
    address:  {
        zip_code: '06233200',
        street_name: 'Av. das Nações Unidas',
        street_number: '3003',
        neighborhood: 'Bonfim',
        city: 'Osasco',
        federal_unit: 'SP'
    }
  }
};

mercadopago.payment.create(payment_data).then(function (data) {

}).catch(function (error) {

});

```
```java
import com.mercadopago.*;

MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

Payment payment = new Payment();

payment.setTransactionAmount(100f)
       .setDescription("Product Title")
       .setPaymentMethodId("bolbradesco")
       .setPayer(new Payer()
           .setEmail("test@test.com")
           .setFirstName("Test")
           .setLastName("User")
           .setIdentification(new Identification()
               .setType("CPF")
               .setNumber("19119119100"))
           .setAddress(new Address()
               .setZipCode("06233200")
               .setStreetName("Av. das Nações Unidas")
               .setStreetNumber(3003)
               .setNeighborhood("Bonfim")
               .setCity("Osasco")
               .setFederalUnit("SP"))
);

payment.save();
```
```ruby
require 'mercadopago'
sdk = Mercadopago::SDK.new('ENV_ACCESS_TOKEN')

payment_request = {
  transaction_amount: 100,
  description: "Product Title",
  payment_method_id: "bolbradesco",
  payer: {
    email: 'test@test.com',
    identification: {
      type: 'CPF',
      number: '19119119100',
    }
  }
}

payment_response = sdk.payment.create(payment_request)
payment = payment_response[:response]

```
```csharp

using MercadoPago.Config;
using MercadoPago.Client.Common;
using MercadoPago.Client.Payment;
using MercadoPago.Resource.Payment;

MercadoPagoConfig.AccessToken = "ENV_ACCESS_TOKEN";

var request = new PaymentCreateRequest
{
    TransactionAmount = float.Parse("105"),
    Description = "Product Title",
    PaymentMethodId = "bolbradesco",
    Payer = new Payer(){
        Email = "test@test.com",
        FirstName = "Test",
        LastName = "User",
        Identification = new IdentificationRequest
        {
            Type = "CPF",
            Number = "191191191-00",
        },
    },
};

var client = new PaymentClient();
Payment payment = await client.CreateAsync(request);

```
```python
import mercadopago
sdk = mercadopago.SDK("ENV_ACCESS_TOKEN")

payment_data = {
    "transaction_amount": 100,
    "description": "Título do produto",
    "payment_method_id": "pix",
    "payer": {
        "email": "test@test.com",
        "first_name": "Test",
        "last_name": "User",
        "identification": {
            "type": "CPF",
            "number": "191191191-00"
        },
        "address": {
            "zip_code": "06233-200",
            "street_name": "Av. das Nações Unidas",
            "street_number": "3003",
            "neighborhood": "Bonfim",
            "city": "Osasco",
            "federal_unit": "SP"
        }
    }
}

payment_response = sdk.payment().create(payment_data)
payment = payment_response["response"]
```
```curl
curl -X POST \
    -H 'accept: application/json' \
    -H 'content-type: application/json' \
    -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
    'https://api.mercadopago.com/v1/payments' \
    -d '{
      "transaction_amount": 100,
      "description": "Product Title",
      "payment_method_id": "bolbradesco",
      "payer": {
        "email": "test@test.com",
        "first_name": "Test",
        "last_name": "User",
        "identification": {
            "type": "CPF",
            "number": "19119119100"
        },
        "address": {
            "zip_code": "06233200",
            "street_name": "Av. das Nações Unidas",
            "street_number": "3003",
            "neighborhood": "Bonfim",
            "city": "Osasco",
            "federal_unit": "SP"
        }
      }
    }'
```
]]]

<br>
The response will show pending status until the buyer makes the payment. The payment voucher ID is the same as the Mercado Pago transaction ID.

```json
[
{
    ...,
    "id": 5466310457,
    "status": "pending",
    "status_detail": "pending_waiting_payment",
    ...,
    "transaction_details": {
        "net_received_amount": 0,
        "total_paid_amount": 100,
        "overpaid_amount": 0,
        "external_resource_url": "https://www.mercadopago.com/mlb/payments/ticket/helper?payment_id=123456789&payment_method_reference_id= 123456789&caller_id=123456",
        "installment_amount": 0,
        "financial_institution": null,
        "payment_method_reference_id": "1234567890"
    }
}
]
```

In the `external_resource_url` field you will find an address with payment instructions for your buyer. You can redirect or send him/her the link.

> NOTE
>
> Note
>
> Customers have 3 to 5 days to pay, depending on the payment method. After that, you should cancel it.

## Payments date of expiration

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Pix payments

By default, Pix payments expire in 24 hours. You can change this field `date_of_expiration` when creating the payment. The set date should be between 1 and 30 days from issue date.

[[[
```php
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===
$payment->date_of_expiration = "2020-05-30T23:59:59.000-04:00";
```
```node
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===
date_of_expiration: "2020-05-30T23:59:59.000-04:00",
```
```java
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===
payment.setDateOfExpiration("2020-05-30T23:59:59.000-04:00")
```
```ruby
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===
date_of_expiration: '2020-05-30T23:59:59.000-04:00',
```
```csharp
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===
paymentCreateRequest.DateOfExpiration = DateTime.Parse("2020-05-30T23:59:59.000-04:00");
```
```python
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

"date_of_expiration": "2020-05-30T23:59:59.000-04:00"
```
```curl
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===
"date_of_expiration": "2020-05-30T23:59:59.000-04:00",
```
]]]

> WARNING
>
> Important
>
> If you try to make the payment after the date of expiration set, the transaction will be rejected.

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Boleto payments

The default expiration date for boleto payments is 3 days. If you want, you can change this date by sending the `date_of_expiration` field in the payment creation request. The configured date must be between 1 and 30 days from the issue date.

[[[
```php
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===
$payment->date_of_expiration = "2020-05-30T23:59:59.000-04:00";
```
```node
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===
date_of_expiration: "2020-05-30T23:59:59.000-04:00",
```
```java
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===
payment.setDateOfExpiration("2020-05-30T23:59:59.000-04:00")
```
```ruby
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===
date_of_expiration: '2020-05-30T23:59:59.000-04:00',
```
```csharp
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===
paymentCreateRequest.DateOfExpiration = DateTime.Parse("2020-05-30T23:59:59.000-04:00");
```
```python
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===

"date_of_expiration": "2020-05-30T23:59:59.000-04:00"
```
```curl
===
The date uses the ISO 8601 format: yyyy-MM-dd'T'HH:mm:ssz
===
"date_of_expiration": "2020-05-30T23:59:59.000-04:00",
```
]]]

The deadline for approval of the boleto is up to 48 working hours. Therefore, we recommend that you set the due date with at least 3 days to ensure that payment is made.

> WARNING
>
> Important
>
> If the boleto is paid after the expiration date, the amount will be refunded to the payer's Mercado Pago account.

## Cancel payments

To avoid collection issues, you need to cancel expired payments. Cash payments should be paid within 3 to 5 business days, based on their relevant term.

Take into account that **you can only cancel payments in process or pending status**. If a payment expires after 30 days, the cancellation is automatic and the final status will be cancelled or expired.

For more information, check the [Refunds and Cancellations section](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/manage-account/account/cancellations-and-refunds).

## Payment credit times

Each payment method has its own credit times; it is immediate in some cases, while in others, it may take up to 3 business days.

Check [credit times by payment method](https://www.mercadopago.com.br/ajuda/meios-de-pagamento-parcelamento_265) whenever you need to.

------------

---
### Next steps

> LEFT_BUTTON_RECOMMENDED_EN
>
> Test your integration
>
> Check that everything works in your integration with test users.
>
> [Test your integration](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/testing)

> RIGHT_BUTTON_RECOMMENDED_EN
>
> Advanced integration
>
> Enhance your integration and improve your sales management.
>
> [Advanced integration](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/advanced-integration)
