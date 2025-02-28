# Prerequisites to get started

Find out what you need to know to integrate our Checkout API.

## Have your credentials handy
Your credentials are the **keys we provide you to configure your integrations**. In this case, you will use a public and a private key.

* The Public Key grants access to the resources that your frontend needs. It allows you to collect credit card data and turn it into a representative token that you can send to your servers securely to create a payment.

* The private key, or Access Token, will allow you to call the rest of the APIs from your servers. For example, to process a payment, make a refund, or save cards.

To find them, go to the [Credentials section]([FAKER][CREDENTIALS][URL]).

> Do you have any questions about credentials? Check our [FAQs](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/faqs/credentials).


## Always use our libraries
Remember to always use our official libraries for your integration. This helps protect your customer's sensitive data, meet the required security standards, and keep yourself up to date.

<br>

> CLIENT_SIDE
>
> h3
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Include MercadoPago.js

MercadoPago.js allows you to create a payment token to send card data to your backend securely. You can use it by adding the following code to your website:

```html
<script src="https://secure.mlstatic.com/sdk/javascript/v1/mercadopago.js"></script>
```

<br>

> SERVER_SIDE
>
> h3
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Install Mercado Pago SDK

Install an [official SDK](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/sdks) to simplify your interaction with our APIs.

[[[
```php
===
[Install Composer](https://getcomposer.org/download) to use the SDK.

Then run the following code on the command line:
===
php composer.phar require "mercadopago/dx-php:dev-master"
```
```node
===
To install the SDK you must execute the following code on the command line of your terminal using [npm](https://www.npmjs.com/get-npm):
===
npm install mercadopago
```
```java
===
To install the SDK in your [Maven](http://maven.apache.org/install.html) project, add the following dependency in your pom.xml file and then run ´maven install´.
===
<dependency>
            <groupId> com.mercadopago </groupId>
            <artifactId> sdk-java </artifactId>
            <version> 1.8.0 </version>
</dependency>
```
```ruby
===
The Mercado Pago SDK is available as a [gem](https://rubygems.org/gems/mercadopago-sdk), to install it you must execute the following code on the command line:
===
gem install mercadopago-sdk
```
```csharp
===
----[mlb]----
Use [NuGet](https://docs.microsoft.com/pt-br/nuget/reference/nuget-exe-cli-reference) to install the Mercado Pago SDK .NET.
------------
----[mla, mco, mlu, mlc, mlm]----
Use [NuGet](https://docs.microsoft.com/es-es/nuget/reference/nuget-exe-cli-reference) to install the Mercado Pago SDK .NET.
------------
To do this, run the following command on your terminal:
===
nuget install mercadopago-sdk
```
```python
===
The Mercado Pago SDK is available as a [pip](https://pypi.org/project/mercadopago/), to install it you must execute the following code on the command line:
===
pip3 install mercadopago
```
]]]

## Learn our API References

If you can't use our official SDKs, you will have all the information about data queries and responses available to interact directly with our APIs in [API References](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/reference/payments/_payments/post).

## Meet the requirements to go to production

When you develop your integration, consider all the [requirements to go to production](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/goto-production). This process can ensure your customer's data security, compliance with legal standards or provisions in each country, and the best shopping experience for your customers.

Once your integration is ready, you just need to fill out the form for to go to production in the [Credentials section]([FAKER][CREDENTIALS][URL]) to start receiving payments.


---
### Next steps

> LEFT_BUTTON_REQUIRED_EN
>
> Integrate Checkout API for cards
>
> Create and configure your own payment experience.
>
> [Integrate Checkout API for cards](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/online-payments/checkout-api/receiving-payment-by-card)


> RIGHT_BUTTON_RECOMMENDED_EN
>
> API References
>
> Find all the information required to interact with our APIs.
>
> [API References](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/reference)
