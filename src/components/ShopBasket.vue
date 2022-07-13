<template>
  <section class="shop-cart">
    <h3 class="title">購物籃</h3>
    <div class="card-list scrollbar">
      <div
        v-for="product in products"
        :key="product.id"
        class="cart-item d-flex justify-content-between"
      >
        <img :src="product.picture" alt="" />
        <div class="content d-flex flex-column justify-content-between">
          <div class="info">
            <div class="name">{{ product.name }}</div>
            <div class="buy-num">
              <i @click="cutNum(product.id)" class="fa-solid fa-minus icon"></i>
              <span>{{ product.count }}</span>
              <i @click="plusNum(product.id)" class="fa-solid fa-plus icon"></i>
            </div>
          </div>
          <div class="price">$ {{ product.newPrice }}</div>
        </div>
      </div>
    </div>
    <div class="bill">
      <div class="bill-part d-flex justify-content-between">
        <p class="text">運費</p>
        <p class="freight price">{{ freight }}</p>
      </div>
      <div class="bill-part d-flex justify-content-between">
        <p class="text">小計</p>
        <p class="total price">$ {{ sumTotal }}</p>
      </div>
    </div>
  </section>
</template>

<script>
const dummyData = {
  products: [
    {
      id: 1,
      name: "破壞補丁修身牛仔褲",
      count: 1,
      price: 3299,
      picture: "https://i.postimg.cc/wTmND7NV/Block-2x.png",
    },
    {
      id: 2,
      name: "刷色直筒牛仔褲",
      count: 1,
      price: 1299,
      picture: "https://i.postimg.cc/ZRhc92VT/Block-2x-1.png",
    },
    {
      id: 3,
      name: "破壞補丁修身牛仔褲",
      count: 1,
      price: 3299,
      picture: "https://i.postimg.cc/wTmND7NV/Block-2x.png",
    },
    {
      id: 4,
      name: "刷色直筒牛仔褲",
      count: 1,
      price: 1299,
      picture: "https://i.postimg.cc/ZRhc92VT/Block-2x-1.png",
    },
    {
      id: 5,
      name: "破壞補丁修身牛仔褲",
      count: 1,
      price: 3299,
      picture: "https://i.postimg.cc/wTmND7NV/Block-2x.png",
    },
  ],
};

export default {
  name: "ShopBasket",
  props: {
    shipping_fee: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      products: [],
      freight: "免費",
    };
  },
  created() {
    this.fetchProduct();
  },
  methods: {
    fetchProduct() {
      this.products = dummyData.products.map((product) => ({
        ...product,
        newPrice: product.price,
      }));
    },
    plusNum(productId) {
      this.products = this.products.map((product) => {
        if (product.id === productId) {
          return {
            ...product,
            count: product.count + 1,
            newPrice: (product.newPrice += product.price),
          };
        }
        return product;
      });
    },
    cutNum(productId) {
      this.products = this.products.map((product) => {
        if (product.id === productId && product.count > 1) {
          return {
            ...product,
            count: product.count - 1,
            newPrice: (product.newPrice -= product.price),
          };
        } else {
          return product;
        }
      });
    },
  },
  computed: {
    sumTotal() {
      let total = 0;
      this.products.forEach((product) => {
        total += product.newPrice;
      });
      // 針對 運費做運算處理
      if (this.freight !== "免費") {
        total += Number(this.freight.split("").slice(1).join("").trim());
      }
      this.$emit("after-total", total);
      return total;
    },
  },
  watch: {
    shipping_fee: {
      handler: function () {
        this.freight = this.shipping_fee.toString();
      },
      Immediate: true,
    },
  },
};
</script>

<style lang="scss" scoped>
.scrollbar {
  &::-webkit-scrollbar {
    width: 6px;
  }

  // bar的樣式
  &::-webkit-scrollbar-thumb {
    border-radius: 3px;
    background-color: rgba($color: #808080, $alpha: 1);
  }
}

.shop-cart {
  margin: 32px 0;
  padding: 0 16px;
  border: 1px solid #f0f0f5;
  border-radius: 8px;
  background-color: var(--cart-background-color);

  img {
    width: 100px;
    height: 100px;
    border-radius: 4px;
  }

  h3.title {
    display: none;
  }

  .price {
    font-weight: 700;
  }

  // card- list
  .card-list {
    height: calc(65vh - 165px);
    min-height: 350px;
    overflow-y: scroll;
    margin-top: 18px;
  }

  .cart-item {
    margin-bottom: 18px;
    padding-right: 10px;

    .content {
      text-align: end;

      .buy-num {
        margin-top: 18px;

        span {
          display: inline-block;
          width: 50px;
          text-align: center;
        }

        .icon {
          text-align: center;
          width: 26px;
          line-height: 26px;
          border-radius: 50%;
          background-color: var(--mathIcon-background-color);
        }
      }
    }
  }

  // bill
  .bill {
    margin: 16px 0;
  }

  .bill-part {
    padding: 16px 0;
    border-top: 1px solid #f0f0f5;

    .text {
      font-size: 14px;
    }
  }
}
@media screen and (min-width: 375px) {
  // shop-cart
  .shop-cart {
    grid-column: 8/13;
    grid-row: 2/3;
    margin: 24px 0;

    .title {
      display: block;
      padding-top: 32px;
    }
  }
}
</style>