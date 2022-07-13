<template>
  <div id="app">
    <NavTabs />
    <router-view />
    <Footer />
  </div>
</template>

<script>
import NavTabs from "./components/NavTabs.vue";
import Footer from "./components/Footer.vue";
export default {
  components: {
    NavTabs,
    Footer,
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;700&family=Roboto:wght@300;400;500;700&display=swap");

// reset
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body {
  width: 100%;
  height: 100%;
  font-size: 16px;
  font-family: "Noto Sans TC", sans-serif;
  font-family: "Roboto", sans-serif;
}

ol,
ul {
  list-style: none;
}

a {
  text-decoration: none;
  color: var(--main-font-color);
}

// global
.container {
  max-width: 90%;
  margin: 0 auto;
}

.d-none {
  display: none !important;
}

.d-inline {
  display: inline;
}

.d-inline-block {
  display: inline-block;
}

.d-block {
  display: block;
}

.d-flex {
  display: flex;
}

.flex-column {
  flex-direction: column;
}

.justify-content-start {
  justify-content: flex-start;
}

.justify-content-end {
  justify-content: flex-end;
}

.justify-content-center {
  justify-content: center;
}

.justify-content-between {
  justify-content: space-between;
}

.justify-content-around {
  justify-content: space-around;
}

.align-content-start {
  align-content: flex-start;
}

.align-content-end {
  align-content: flex-end;
}

.align-content-center {
  align-content: center;
}

.align-items-start {
  align-items: flex-start;
}

.align-items-end {
  align-items: flex-end;
}

.align-items-center {
  align-items: center;
}

.text-center {
  text-align: center;
}

.text-right {
  text-align: right;
}

.text-left {
  text-align: left;
}

.cursor-pointer {
  cursor: pointer;
}

.w-50 {
  width: 50%;
}

.w-100 {
  width: 100%;
}
</style>
