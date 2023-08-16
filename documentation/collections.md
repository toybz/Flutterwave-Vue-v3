<p align="center">
    <img title="Flutterwave" height="200" src="https://flutterwave.com/images/logo/full.svg" width="50%"/>
</p>

# Collections

We recommend reading the [main readme](/README.md) first, to understand the requirements for using the library and how to initiate this in your apps. This guide assumes you've read that.

You can generate a payment modal to collect payments from your users using either the in-built component or the global methods, and optionally specify an action that would run when the payment modal is closed:
1. [Using as a component](#flutterwave-component)
2. [Calling the Flutterwave method](#flutterwave-method)
3. [Closing the payment modal](#close-payment-modal)

Read more about the underlying infrastructure [here](https://developer.flutterwave.com/docs/collecting-payments/inline).


## Flutterwave Component

This section describes how you can utilize the in-built Flutterwave component to generate a payment modal and collect payments from your users.

**Method 1** 

```html
<!--
Method 1: Pass  in payment parameters individually as component attributes
-->

<template>
  <div>
    <flutterwave-pay-button
      :tx_ref="generateReference()"
      amount="20"
      currency="NGN"
      payment_options="card,ussd"
      redirect_url=""
      class="class-name"
      style=""
      :meta="{
        counsumer_id: '7898',
        consumer_mac: 'kjs9s8ss7dd'
      }"
      :customer="{
        name: 'Demo Customer  Name',
        email: 'customer@mail.com', 
        phone_number: '0818450****'
      }"
      :customizations="{
        title: 'Customization Title',
        description: 'Customization Description',
        logo : 'https://flutterwave.com/images/logo-colored.svg'
      }"
      :callback="makePaymentCallback"
      :onclose="closedPaymentModal"
    > Click To Pay </flutterwave-pay-button>
  </div>
</template>

<script>
import {FlutterwavePayButton} from "flutterwave-vue-v3";

export default {
  name: "App",
  components: { FlutterwavePayButton },
  methods: {
    makePaymentCallback(response) {
      console.log("Payment callback", response);
    },
    closedPaymentModal() {
      console.log('payment modal is closed');
    },
    generateReference(){
      let date = new Date();
      return date.getTime().toString();
    }
  }
}
</script>

```

**Method 2**

```html
<!--
Method 2: Pass  in payment parameters as object to v-bind
-->

<template>
  <div>
    <flutterwave-pay-button   v-bind="paymentData" >Click To Pay</flutterwave-pay-button>
 </div>
</template>

<script>
export default {
  name: "App",
  data(){
    return {
      paymentData: {
        tx_ref: this.generateReference(),
        amount: "10",
        currency: "NGN",
        payment_options: "card,ussd",
        redirect_url: "",
        meta: {
          counsumer_id: "7898",
          consumer_mac: "kjs9s8ss7dd"
        },
        customer: {
          name: "Demo Customer  Name",
          email: "customer@mail.com",
          phone_number: "0818450***44"
        } ,
        customizations: {
          title: "Customization Title",
          description: "Customization Description",
          logo: "https://flutterwave.com/images/logo-colored.svg"
        },
        callback: this.makePaymentCallback,
        onclose: this.closedPaymentModal
      }
    }
  } ,
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


## Flutterwave method

This section describes how you can utilize the global Flutterwave method to generate a payment modal and collect payments from your users.

**Using the '$payWithFlutterwave()' method**

```html
<!--
Method 1: Utilize the synchronous method
-->

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
        amount: "10",
        currency: "NGN",
        payment_options: "card,ussd",
        redirect_url: "",
        meta: {
          counsumer_id: "7898",
          consumer_mac: "kjs9s8ss7dd"
        },
        customer: {
          name: "Demo Customer  Name",
          email: "customer@mail.com",
          phone_number: "081845***044"
        } ,
        customizations: {
          title: "Customization Title",
          description: "Customization Description",
          logo: "https://flutterwave.com/images/logo-colored.svg"
        },
        callback: this.makePaymentCallback,
        onclose: this.closedPaymentModal
      }
    }
  } ,
  methods: {
    payViaService() {
      this.$payWithFlutterwave(this.paymentData);
    } ,
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

**Using the '$asyncPayWithFlutterwave()' method**

```html
<!--
Method 2: Utilize the asynchronous method (non-blocking)
-->

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
        amount: "10",
        currency: "NGN",
        payment_options: "card,ussd",
        redirect_url: "",
        meta: {
          counsumer_id: "7898",
          consumer_mac: "kjs9s8ss7dd"
        },
        customer: {
          name: "Demo Customer  Name",
          email: "customer@mail.com",
          phone_number: "081845***044"
        },
        customizations: {
          title: "Customization Title",
          description: "Customization Description",
          logo: "https://flutterwave.com/images/logo-colored.svg"
        },
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


## Close payment modal

This section describes how you can optionally specify an action that runs when the payment modal is closed.

**Using the "$closePaymentModal()" method**

```html
<template>

  <div>
    <flutterwave-pay-button
      :tx_ref="generateReference()"
      amount="20"
      currency="NGN"
      payment_options="card,ussd"
      redirect_url=""
      class="class-name"
      style=""
      :meta="{
        counsumer_id: '7898',
        consumer_mac: 'kjs9s8ss7dd'
      }"
      :customer="{
        name: 'Demo Customer  Name',
        email: 'customer@mail.com', 
        phone_number: '0818450****'
      }"
      :customizations="{
        title: 'Customization Title',
        description: 'Customization Description'  ,
        logo : 'https://flutterwave.com/images/logo-colored.svg'
      }"
      :callback="makePaymentCallback"
      :onclose="closePaymentCallback"
    >
      Click To Pay
    </flutterwave-pay-button>
  </div>

</template>

<script>
import {FlutterwavePayButton} from "flutterwave-vue-v3";

export default {
  name: "App",
  components: { FlutterwavePayButton },
  methods: {
    makePaymentCallback(response) {
      console.log("Payment callback", response);
      // Close modal in payment callback
      this.$closePaymentModal();
    },
    closePaymentCallback() {
      console.log('payment modal is closed');
    },
    generateReference(){
      let date = new Date();
      return date.getTime().toString();
    }
  }
}
</script>

```
