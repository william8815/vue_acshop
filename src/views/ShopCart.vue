<template>
  <div class="container">
    <!-- main -->
    <main>
      <h1 class="title">結帳</h1>
      <!-- flow -->
      <ShopCheckout
        @after-address="handleAfterAddress"
        @after-freight="handleAfterFreight"
        @after-payment="handleAfterPayment"
        @after-radio="handleAfterRadio"
      />
      <!-- shop-cart -->
      <ShopBasket
        :shipping_fee="checkoutList.shippingFee"
        @after-total="handleAfterTotal"
      />
    </main>
    <div v-if="isSubmit" @click="hiddenBoard" class="show-board">
      <div class="modal">
        <pre>{{ outputJson }}</pre>
      </div>
    </div>
  </div>
</template>

<script>
import ShopCheckout from "./../components/ShopCheckout.vue";
import ShopBasket from "./../components/ShopBasket.vue";

export default {
  name: "ShopCart",
  components: {
    ShopCheckout,
    ShopBasket,
  },
  data() {
    return {
      checkoutList: {
        salutation: "",
        username: "",
        phone: "",
        email: "",
        city: "",
        addr: "",
        shippingFee: "",
        ccname: "",
        cardnumber: "",
        exdate: "",
        cw: "",
        totalPrice: 0,
      },
      finalOutput: {},
      outputJson: "",
      isSubmit: false,
    };
  },
  methods: {
    handleAfterAddress(formData) {
      for (let [name, value] of formData.entries()) {
        this.finalOutput[name] = value;
      }
      // localStorage.setItem("formData", JSON.stringify(this.finalOutput));
    },
    handleAfterFreight() {
      this.finalOutput = {
        ...this.finalOutput,
        shippingFee: Number(
          this.checkoutList.shippingFee.split("").slice(1).join("").trim()
        ),
      };
    },
    handleAfterPayment(formData) {
      for (let [name, value] of formData.entries()) {
        this.finalOutput[name] = value;
      }
      // localStorage.setItem("formData", JSON.stringify(this.finalOutput));
      this.changeDataType();
      this.isSubmit = true;
    },
    handleAfterRadio(text) {
      this.checkoutList.shippingFee = text;
    },
    handleAfterTotal(total) {
      this.checkoutList.totalPrice = total;
    },
    changeDataType() {
      this.outputJson = JSON.stringify(this.finalOutput, null, "\t");
      console.log(this.outputJson);
    },
    hiddenBoard() {
      this.isSubmit = false;
    },
  },
};
</script>

<style lang="scss" >
// main
.show-board {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background-color: rgba(#333, 0.3);
  z-index: 3;
  .modal {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 50vh auto;
    width: 500px;
    height: 450px;
    transform: translateY(-50%);
    background-color: #fff;
    border-radius: 10px;
  }
}
main {
  width: 100%;
  margin: 0 auto;

  h1.title {
    margin-top: 16px;
  }
}

.button-pannel {
  border-top: 1px solid #e6e6eb;
  padding: 24px 0;

  .btn {
    border: none;
    background-color: transparent;
    font-size: 14px;
    cursor: pointer;
  }

  .btn-prev {
    color: var(--main-font-color);

    &::before {
      content: "\2190";
      font-size: 20px;
      margin-right: 13.5px;
    }
  }

  .btn-next {
    width: 100%;
    height: 46px;
    border-radius: 8px;
    background-color: #f67599;
    color: #fff;

    &::after {
      content: "\2192";
      font-size: 20px;
      margin-left: 13.5px;
    }

    &.no-after::after {
      display: none;
    }
  }
}
@media screen and (min-width: 375px) {
  // main
  main {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    grid-template-rows: auto minmax(1fr, 600px);

    .title {
      grid-column: 1/13;
      grid-row: 1/2;
    }

    // button
    .button-pannel {
      margin-top: 48px;

      .btn-next {
        width: 50%;
        max-width: 174px;
      }
    }
  }
}
</style>