<template>
  <section class="flow">
    <!-- stepper -->
    <div class="stepper">
      <div class="step" :class="{ active: counter === 0, focus: counter > 0 }">
        <div class="circle"></div>
        <div class="text">寄送地址</div>
        <div class="line"></div>
      </div>
      <div class="step" :class="{ active: counter === 1, focus: counter > 1 }">
        <div class="circle"></div>
        <div class="text">運送方式</div>
        <div class="line"></div>
      </div>
      <div class="step" :class="{ active: counter === 2 }">
        <div class="circle"></div>
        <div class="text">付款資訊</div>
      </div>
    </div>
    <!-- form -->
    <div class="form">
      <div class="form-input">
        <!-- 寄送地址 -->
        <form
          @submit.stop.prevent="handleAddressSubmit"
          v-show="counter === 0"
          id="Address"
        >
          <div class="part address">
            <h1>寄送地址</h1>
            <div class="title">
              <label for="salutation">稱謂</label>
              <div class="selector">
                <select
                  v-model="formOneData.salutation"
                  id="salutation"
                  name="salutation"
                  required
                >
                  <option value="" selected hidden disabled>稱謂</option>
                  <option value="Mr.">先生</option>
                  <option value="Ms.">小姐</option>
                </select>
              </div>
            </div>
            <div class="name">
              <label for="name">姓名</label>
              <input
                v-model="formOneData.username"
                type="text"
                id="name"
                name="username"
                placeholder="請輸入姓名"
                required
              />
            </div>
            <div class="phone-number">
              <label for="phoneNumber">電話</label>
              <input
                v-model="formOneData.phone"
                type="text"
                id="phoneNumber"
                name="phone"
                placeholder="請輸入行動電話"
                required
              />
            </div>
            <div class="email">
              <label for="email">Email</label>
              <input
                v-model="formOneData.email"
                type="email"
                id="email"
                name="email"
                placeholder="請輸入電子郵件"
                required="required"
              />
            </div>
            <div class="capital">
              <label for="city">縣市</label>
              <div class="selector">
                <select
                  v-model="formOneData.city"
                  id="city"
                  name="city"
                  required
                >
                  <option value="" selected hidden disabled>請選擇縣市</option>
                  <option value="台北">台北</option>
                  <option value="台中">台中</option>
                  <option value="台南">台南</option>
                  <option value="高雄">高雄</option>
                </select>
              </div>
            </div>
            <div class="location">
              <label for="addr">地址</label>
              <input
                v-model="formOneData.addr"
                type="text"
                id="addr"
                name="addr"
                placeholder="請輸入地址"
                required
              />
            </div>
          </div>
        </form>
        <!-- 運送方式 -->
        <form
          @submit.stop.prevent="handleFreightSubmit"
          v-show="counter === 1"
          id="freightWay"
        >
          <div class="part transport-way">
            <h1>運送方式</h1>
            <div
              @click="selectFreight(deliveryItem.id)"
              v-for="deliveryItem in delivery"
              :key="deliveryItem.id"
              :class="{ checked: deliveryItem.isChecked }"
              class="way"
              style="margin-top: 24px"
            >
              <input
                :value="deliveryItem.costText"
                :checked="
                  handleRadio(deliveryItem.isChecked, deliveryItem.costText)
                "
                type="radio"
                name="shippingFee"
                class="radio"
              />
              <label for="">
                <h3 class="transport-title">{{ deliveryItem.title }}</h3>
                <p class="transport-time">{{ deliveryItem.time }}</p>
              </label>
              <p class="transport-cost">{{ deliveryItem.costText }}</p>
            </div>
          </div>
        </form>
        <!-- 付款資訊 -->
        <form
          @submit.stop.prevent="handlePaymentSubmit"
          v-show="counter === 2"
          id="paymentInfo"
        >
          <div class="part payment">
            <h1>付款資訊</h1>
            <div class="user">
              <label for="ccname">持卡人姓名</label>
              <input
                v-model="formThirdData.ccname"
                type="text"
                id="ccname"
                name="ccname"
                placeholder="John Doe"
                class="user-name"
                required
              />
            </div>
            <div class="card-number">
              <label for="cardNumber">卡號</label>
              <input
                v-model="formThirdData.cardnumber"
                type="text"
                placeholder="1111 2222 3333 4444"
                class="number"
                id="cardNumber"
                name="cardNumber"
                required
              />
            </div>
            <div class="card-date">
              <label for="cardDate">有效期限</label>
              <input
                v-model="formThirdData.exdate"
                type="date"
                id="cardDate"
                name="exdate"
                placeholder="MM/YY"
                required
              />
            </div>
            <div class="card-security">
              <label for="cardSecurity">CVC/CCV</label>
              <input
                v-model="formThirdData.cw"
                type="text"
                id="cardSecurity"
                name="cw"
                placeholder="123"
                required
              />
            </div>
          </div>
        </form>
      </div>
      <div class="form-submit">
        <!-- button -->
        <div class="button-pannel d-flex justify-content-between">
          <span v-if="counter < 1"></span>
          <button v-else @click.prevent.stop="prevStep" class="btn btn-prev">
            上一步
          </button>

          <button
            @click.stop="nextStep"
            type="submit"
            :form="changeForm"
            class="btn btn-next"
          >
            {{ buttonText }}
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
const dummyDelivery = {
  delivery: [
    {
      id: 1,
      title: "標準運送",
      time: "約 3~7 個工作天",
      costText: "免費",
      cost: 0,
      isChecked: true,
    },
    {
      id: 2,
      title: "DHL 貨運",
      time: "48 小時內送達",
      costText: "$ 500",
      cost: 500,
      isChecked: false,
    },
  ],
};
export default {
  name: "ShopCheckout",
  data() {
    return {
      counter: 0,
      delivery: [],
      formOneData: {
        salutation: "",
        username: "",
        phone: "",
        email: "",
        city: "",
        addr: "",
      },
      formSecData: {
        shippingFee: "",
      },
      formThirdData: {
        ccname: "",
        cardnumber: "",
        exdate: "",
        cw: "",
      },
      currentForm: {},
      totalFrom: {
        salutation: "",
        username: "",
        phone: "",
        email: "",
        city: "",
        addr: "",
        shippingFee: "免費",
        ccname: "",
        cardnumber: "",
        exdate: "",
        cw: "",
      },
    };
  },
  created() {
    this.fetchDelivery();
    this.fetchForm();
  },
  methods: {
    fetchDelivery() {
      this.delivery = dummyDelivery.delivery;
    },
    fetchForm() {
      const jsonForm =
        JSON.parse(localStorage.getItem("formData")) || this.totalFrom;
      // console.log(jsonForm);
      const { salutation, username, phone, email, city, addr } = jsonForm;
      const { shippingFee } = jsonForm;
      const { ccname, cardnumber, exdate, cw } = jsonForm;
      this.formOneData = {
        salutation,
        username,
        phone,
        email,
        city,
        addr,
      };
      this.formSecData = {
        shippingFee,
      };
      this.delivery = this.delivery.map((item) => {
        if (item.costText === this.formSecData.shippingFee) {
          return {
            ...item,
            isChecked: true,
          };
        } else {
          return {
            ...item,
            isChecked: false,
          };
        }
      });
      this.formThirdData = {
        ccname,
        cardnumber,
        exdate,
        cw,
      };
    },
    // 運送方式樣式處理
    selectFreight(deliveryId) {
      this.delivery = this.delivery.map((item) => {
        if (item.id === deliveryId) {
          return {
            ...item,
            isChecked: true,
          };
        } else {
          return {
            ...item,
            isChecked: false,
          };
        }
      });
    },
    // 處理 radio btn 值，並回傳給父元件
    handleRadio(check, text) {
      if (check) {
        this.formSecData.shippingFee = text;
        this.$emit("after-radio", text);
        return true;
      }
      return false;
    },
    // 處理 各表單，並傳回父元件
    handleAddressSubmit(e) {
      const form = e.target;
      // formData : 將表單整個資料整合包起來，可傳送給後端伺服器
      const formData = new FormData(form);
      this.$emit("after-address", formData);
    },
    handleFreightSubmit(e) {
      const form = e.target;
      const formData = new FormData(form);
      this.$emit("after-freight", formData);
    },
    handlePaymentSubmit(e) {
      const form = e.target;
      const formData = new FormData(form);
      this.$emit("after-payment", formData);
    },
    // nextStep 、prevStep 為counter計算器
    nextStep() {
      if (this.counter === 2) {
        for (let key in this.formThirdData) {
          if (this.formThirdData[key] === "") {
            return;
          }
        }
      }
      if (this.counter === 1) {
        for (let key in this.formSecData) {
          if (this.formSecData[key] === "") {
            return;
          }
        }
        setTimeout(() => {
          this.counter += 1;
        }, 1);
      }
      if (this.counter === 0) {
        for (let key in this.formOneData) {
          if (this.formOneData[key] === "") {
            return;
          }
        }
        setTimeout(() => {
          this.counter += 1;
        }, 1);
      }

      if (this.counter > 2) {
        this.counter = 2;
        return;
      }
    },
    prevStep() {
      this.counter -= 1;
      if (this.counter < 0) {
        this.counter = 0;
        return;
      }
    },
  },
  computed: {
    buttonText() {
      return this.counter < 2 ? "下一步" : "確認訂單";
    },
    // 切換目標表單
    changeForm() {
      if (this.counter === 0) {
        return "Address";
      } else if (this.counter === 1) {
        return "freightWay";
      } else {
        return "paymentInfo";
      }
    },
  },
  watch: {
    formOneData: {
      handler: function (tempObj) {
        this.totalFrom = {
          ...this.totalFrom,
          ...tempObj,
        };
        localStorage.setItem("formData", JSON.stringify(this.totalFrom));
      },
      deep: true,
    },
    formSecData: {
      handler: function (tempObj) {
        this.totalFrom = {
          ...this.totalFrom,
          ...tempObj,
        };
        localStorage.setItem("formData", JSON.stringify(this.totalFrom));
      },
      deep: true,
    },
    formThirdData: {
      handler: function (tempObj) {
        this.totalFrom = {
          ...this.totalFrom,
          ...tempObj,
        };
        localStorage.setItem("formData", JSON.stringify(this.totalFrom));
      },
      deep: true,
    },
  },
};
</script>


<style lang="scss" scoped>
// stepper
@mixin getStepNums($num) {
  @for $i from 1 through $num {
    &:nth-child(#{$i}) .circle::after {
      content: "#{$i}";
    }
  }
}
.stepper {
  display: grid;
  grid-template-columns: repeat(2, 1fr) auto;
  margin: 24px 0;

  .step {
    position: relative;
    display: flex;
    align-items: center;
    @include getStepNums(3);

    .circle {
      width: 32px;
      line-height: 32px;
      border-radius: 50%;
      border: 1px solid #afb1bd;
      color: var(--step-font-color);
      text-align: center;
    }

    .text {
      display: none;
    }

    .line {
      height: 2px;
      background-color: #f0f0f5;
      position: absolute;
      top: 50%;
      right: 32px;
      left: 64px;
    }

    &.active,
    &.focus {
      .circle {
        background-color: var(--stepper-background-color);
        color: #fff;
      }

      .line {
        background-color: var(--stepper-background-color);
      }
    }

    &.focus .circle::after {
      background-color: #333;
      content: "\2714";
    }
  }
}

// form
%input-style {
  display: block;
  width: 100%;
  height: 40px;
  border-radius: 4px;
  border: none;
  border: 1px solid #4a4a4a;
  padding: 12px 0 12px 16px;
  margin-top: 8px;
}

%label-style {
  font-weight: 700;
  font-size: 12px;
  line-height: 16px;
  color: #808080;
}

.form {
  height: calc(100% - 82px);
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 1fr auto;
  input[type="text"] {
    @extend %input-style;
    background-color: transparent;
  }
  input[type="email"] {
    @extend %input-style;
    background-color: transparent;
  }
  input[type="date"] {
    @extend %input-style;
    background-color: transparent;
    padding-right: 16px;
  }

  input[type="radio"] {
    -webkit-appearance: none;
    width: 20px;
    height: 20px;
    border: 1px solid #222222;
    border-radius: 50%;

    &:checked {
      box-shadow: inset 0 0 0 6px var(--box-shadow-inset);
    }
  }

  select {
    @extend %input-style;
    -webkit-appearance: none;
    cursor: pointer;
    background-color: transparent;

    // 改變沒選定時的文字顏色
    &:invalid {
      color: #999;
    }
  }
}

// 寄送地址
.address {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px 30px;
  grid-template-areas:
    "h1 h1 h1 "
    "title user-name user-name"
    "phone phone phone "
    "email email email "
    "capital capital capital "
    "location location location ";

  h1 {
    grid-area: h1;
  }

  .title {
    grid-area: title;
    min-width: 120px;
  }

  .name {
    grid-area: user-name;
  }

  .phone-number {
    grid-area: phone;
  }

  .email {
    grid-area: email;
  }

  .capital {
    grid-area: capital;
  }

  .location {
    grid-area: location;
  }

  label {
    @extend %label-style;
  }

  .selector {
    position: relative;
    background-color: transparent;
    z-index: 0;
    &::after {
      content: "";
      border-style: solid;
      border-width: 6px 5px 0 5px;
      border-color: #2a2a2a transparent transparent transparent;
      position: absolute;
      right: 18px;
      top: 50%;
      transform: translateY(-50%);
      z-index: -1;
    }
  }
}

// 運送方式
.transport-way {
  .way {
    display: grid;
    grid-template-columns: auto auto 1fr;
    align-items: center;
    padding: 11px 20px;
    border: 1px solid var(--way-border-color);
    border-radius: 4px;
    height: 60px;

    label {
      margin-left: 20px;
    }

    h3 {
      font-size: 14px;
    }

    p {
      font-size: 12px;
      margin-top: 2px;
    }

    .transport-cost {
      justify-self: flex-end;
      align-self: flex-start;
    }

    &.checked {
      border: 1px solid var(--way-checked-color);
    }
  }
}

// 付款資訊
.payment {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 30px 24px;
  grid-template-areas:
    "h1 h1"
    "user user"
    "num num"
    "date secure";

  h1 {
    grid-area: h1;
  }

  .user {
    grid-area: user;

    .user-name {
      max-width: 350px;
    }
  }

  .card-number {
    grid-area: num;

    .number {
      max-width: 350px;
    }
  }

  .card-date {
    grid-area: date;
  }

  .card-security {
    grid-area: secure;
  }

  label {
    @extend %label-style;
  }
}
@media screen and (min-width: 375px) {
  .flow {
    grid-column: 1/7;
    grid-row: 2/3;

    // stepper
    .step {
      .text {
        display: block;
        margin-left: 0.5rem;
      }

      .line {
        left: 128px;
      }
    }

    // form
    .address {
      grid-template-columns: repeat(6, 1fr);
      grid-template-areas:
        "h1 h1 h1 h1 h1 h1 "
        "title title user-name user-name user-name user-name"
        "phone phone phone email email email"
        "capital capital location location location location";
    }
  }
}
</style>