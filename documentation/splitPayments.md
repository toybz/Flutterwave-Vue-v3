<p align="center">
    <img title="Flutterwave" height="200" src="https://flutterwave.com/images/logo/full.svg" width="50%"/>
</p>

# Split payments

We recommend reading the [main readme](/README.md) first, to understand the requirements for using the library and how to initiate this in your apps. You would aso need to have previously created a collection subaccount (you can find a guide [here](https://developer.flutterwave.com/docs/collecting-payments/split-payments)). This guide assumes you've read these.

Also, this page simply provides a sample for initiating a split payment when collecting payments from your users. If you would like to view the general steps to generate a payment modal to collect payments from your users, you can refer to the [collections guide](documentation/collections.md).


## Utilizing subaccounts

This describes how to split payments collected from your users. You can choose to either use the split values initially specified when creating the split payment, or you can override the values, and specify new split values for each individual transaction:
1. [Using the defaults](#using-the-defaults)
2. [Overriding the defaults](#overriding-the-defaults)

## Using the defaults

```html
<template>
  <div>
    <flutterwave-pay-button v-bind="paymentData" >Click To Pay</flutterwave-pay-button>
 </div>
</template>

<script>
export default {
  name: "App",
  data(){
    return {
      paymentData: {
        tx_ref: this.generateReference(),
        amount: "1000",
        currency: "NGN",
        subaccounts: [
          {
            id: "RS_FB312AA6C2C84A13421F3079E714F2CB",
          },
          {
            id: "RS_A8EB7D4D9C66C0B1C75014EE67D4D663"
          }
        ],
        redirect_url: "",
        customer: {
          name: "Flutterwave Developers",
          email: "developers@flutterwavego.com",
          phone_number: "09012345678"
        },
        customizations: {
          title: "Customization Title",
          description: "Customization Description",
          logo: "https://flutterwave.com/images/logo-colored.svg"
        },
        callback: this.makePaymentCallback,
        onclose: this.closedPaymentModal
      }
    }
  },
  methods: {
    makePaymentCallback(response) {
      console.log("Pay", response);
    },
    closedPaymentModal() {
      console.log('payment is closed');
    },
    generateReference(){
      let date = new Date();
      return date.getTime().toString();
    }
  }
}
</script>

```


## Overriding the defaults

```html
<template>
  <div>
    <button @click="payViaService">Pay Using Code</button>
  </div>
</template>

<script>
export default {
  name: "App",
  data(){
    return {
      paymentData: {
        tx_ref: this.generateReference(),
        amount: "6000",
        currency: "NGN",
        subaccounts: [
          {
            id: "RS_FB312AA6C2C84A13421F3079E714F2CB",
            // If you want a commission of 20% 0f the settled amount
            // Subaccount gets: (1000 - 14) * 0.8 = 788.8
            // You get: (1000 - 14) * 0.2 = 197.2
            transaction_charge_type: "percentage",
            transaction_charge: 4200,
          },
          {
            id: "RS_A8EB7D4D9C66C0B1C75014EE67D4D663",
            // If you want a commission of 150 naira only
            // Subaccount gets: 1000 - 150 - 14 = 850
            // You get: 150
            transaction_charge_type: "flat",
            transaction_charge: 150,
          },
          {
            id: "RS_235E8F4E92A4048B57EA29B0E1B8F78B",
            // If you want the subaccount to get 600 naira only
            // Subaccount gets: 600
            // You get: 1000 - 600 - 14 = 386
            transaction_charge_type: "flat_subaccount",
            transaction_charge: 4200,
          },
        ],
        redirect_url: "",
        customer: {
          name: "Flutterwave Developers",
          email: "developers@flutterwavego.com",
          phone_number: "09012345678"
        },
        customizations: {
          title: "Customization Title",
          description: "Customization Description",
          logo: "https://flutterwave.com/images/logo-colored.svg"
        },
        callback: this.makePaymentCallback,
        onclose: this.closedPaymentModal
      }
    }
  },
  methods: {
    makePaymentCallback(response) {
      console.log("Pay", response);
    },
    closedPaymentModal() {
      console.log('payment is closed');
    },
    generateReference(){
      let date = new Date();
      return date.getTime().toString();
    }
  }
}
</script>

```
