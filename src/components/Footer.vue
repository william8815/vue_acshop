<template>
  <footer>
    <!-- LOGO -->
    <div class="logo logo-bottom"></div>
    <div class="section service">
      <div class="title">客戶服務</div>
      <p><a href="">運送說明</a></p>
      <p><a href="">退換貨相關</a></p>
      <p><a href="">付款資訊</a></p>
      <p><a href="">FAQ</a></p>
    </div>
    <div class="section about-us">
      <div class="title">關於我們</div>
      <p><a href="">品牌故事</a></p>
      <p><a href="">媒體連繫</a></p>
      <p><a href="">Press kit</a></p>
    </div>
    <div class="section information">
      <div class="title">資訊</div>
      <p><a href="">隱私權政策</a></p>
      <p><a href="">Cookie</a></p>
      <p><a href="">GDPR</a></p>
    </div>
    <div class="section follow-us">
      <div class="title">追蹤 ALPHA Shop</div>
      <p><a href="">+886 02123-45678</a></p>
      <div class="media-app">
        <a href=""><img :src="facebook" alt="FB" class="icon fb" /></a>
        <a href=""><img :src="instagram" alt="IG" class="icon" /></a>
        <a href=""><img :src="wechat" alt="Wechat" class="icon" /></a>
      </div>
    </div>
  </footer>
</template>

<script>
import facebook from "@/assets/img/fb_Icon_social@2x.png";
import instagram from "@/assets/img/IG_Icon_social@2x.png";
import wechat from "@/assets/img/wechat_Icon@2x.png";

export default {
  data() {
    return {
      facebook,
      instagram,
      wechat,
    };
  },
};
</script>

<style lang="scss" scoped>
footer {
  height: 284px;
  margin-top: 120px;
  background-color: var(--footer-background-color);
  display: flex;
  justify-content: space-around;
  align-items: center;

  .title {
    color: var(--sub-font-color);
    font-size: 24px;
    font-weight: 700;
    margin-bottom: 24px;
  }

  p {
    margin-bottom: 8px;
  }

  a {
    color: #808080;
  }

  .section {
    height: calc(100% - 64px);
  }

  .media-app {
    .icon {
      width: 20px;
      height: 20px;
      margin-right: 20px;
    }

    .fb {
      width: 10px;
    }
  }
}
</style>