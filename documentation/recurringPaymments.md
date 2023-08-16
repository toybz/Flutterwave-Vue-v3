<p align="center">
    <img title="Flutterwave" height="200" src="https://flutterwave.com/images/logo/full.svg" width="50%"/>
</p>

# Recurring payments

We recommend reading the [main readme](/README.md) first, to understand the requirements for using the library and how to initiate this in your apps. You would aso need to have previously created a payment plan (you can find a guide [here](https://developer.flutterwave.com/docs/recurring-payments/payment-plans)). This guide assumes you've read these.

Also, this page simply provides a sample for adding a user to a payment plan. If you would like to view the general steps to generate a payment modal to collect payments from your users, you can refer to the [collections guide](documentation/collections.md).


## Adding a customer to a subscription

```html
<template>
  <div>
    <button @click="asyncPay">Pay Using Async method</button>
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
        payment_plan: 3807,
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
    asyncPay() {
      this.$asyncPayWithFlutterwave(this.paymentData).then(
        (response) => {
          console.log(response);
        }
      );
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
