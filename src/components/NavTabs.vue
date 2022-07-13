<template>
  <!-- header -->
  <header>
    <div class="container">
      <!-- 漢堡按鈕 -->
      <input type="checkbox" id="input-toggle" class="input-toggle" />
      <label for="input-toggle" class="toggle-icon">
        <span class="hamburger"></span>
      </label>
      <!-- 導覽列 -->
      <nav class="nav-list">
        <div class="opacity">
          <!-- list -->
          <ul class="list">
            <li class="list-item"><a href="#" class="list-link">男款</a></li>
            <li class="list-item"><a href="#" class="list-link">女款</a></li>
            <li class="list-item">
              <a href="#" class="list-link">最新消息</a>
            </li>
            <li class="list-item">
              <a href="#" class="list-link">客製商品</a>
            </li>
            <li class="list-item">
              <a href="#" class="list-link">聯絡我們</a>
            </li>
          </ul>
          <!-- icons -->
          <div class="icons">
            <div class="icon search">
              <!-- <input type="text" placeholder="search"> -->
              <i class="fa-solid fa-magnifying-glass"></i>
            </div>
            <div class="icon shopping">
              <i class="fa-solid fa-cart-shopping"></i>
            </div>
            <div class="icon moon sidebar__menu__link">
              <label class="sidebar__darkmode__label" for="dark__mode__toggle"
                ><i class="fa-solid fa-moon"></i
              ></label>
              <input
                type="checkbox"
                class="sidebar__darkmode__input"
                id="dark__mode__toggle"
              />
            </div>
          </div>
        </div>
      </nav>
      <!-- LOGO -->
      <div class="logo logo-top"></div>
    </div>
  </header>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped>
header {
  height: 50px;
  text-align: center;
  background-color: var(--main-background-color);
  box-shadow: 3px 0 13px 0 rgba(100, 112, 134, 0.1);
  position: sticky;
  top: 0;
  z-index: 2;

  .container {
    display: grid;
    align-items: center;
    justify-content: center;
    height: 100%;
  }

  .input-toggle {
    display: none;
  }

  .input-toggle:checked ~ .nav-list {
    transform: scaleY(1);
  }

  .input-toggle:checked ~ .nav-list .opacity {
    opacity: 1;
  }

  .toggle-icon {
    position: absolute;
    height: 50px;

    .hamburger,
    .hamburger:before,
    .hamburger::after {
      display: block;
      width: 15px;
      height: 2px;
      background-color: var(--sub-font-color);
    }

    .hamburger {
      position: relative;
      top: 50%;
      transform: translateY(-50%);
    }

    .hamburger::before,
    .hamburger::after {
      content: "";
      position: absolute;
    }

    .hamburger::before {
      bottom: 7px;
      width: 20px;
    }

    .hamburger::after {
      top: 7px;
      width: 10px;
    }
  }

  .nav-list {
    position: absolute;
    left: 0;
    top: 100%;
    width: 100%;
    transform: scaleY(0);
    transform-origin: top;
    transition: transform 0.3s ease-out;
    background-color: var(--main-background-color);
    z-index: 999;

    .opacity {
      opacity: 0;
      transition: transform 0.2s ease-out 0.15s;
    }

    .list {
      width: 90%;
      margin: 0 auto;

      .list-item {
        padding: 24px 0;
        border-bottom: 1px solid rgba(42, 42, 42, 0.1);
      }
    }

    .icons {
      width: 50%;
      margin: 25px auto;
      display: flex;
      justify-content: space-between;

      .icon {
        font-size: 1.25rem;
      }

      .sidebar__darkmode__label {
        background-size: 100% 100%;
        cursor: pointer;
        height: 100%;
        width: 100%;
      }

      .sidebar__darkmode__input {
        display: none;
      }
    }
  }

  img {
    width: 150px;
  }
  .logo {
    background: var(--logo-photo);
    background-size: cover;

    &.logo-top {
      width: 171px;
      height: 30px;
    }

    &.logo-bottom {
      width: 175px;
      height: 31px;
    }
  }
}
@media screen and (min-width: 375px) {
  header {
    .container {
      grid-template-columns: 1fr auto 1fr;
    }

    .toggle-icon {
      display: none;
    }

    .nav-list {
      all: unset;

      .opacity {
        opacity: 1;
      }

      .list {
        all: unset;
        display: flex;

        .list-item {
          all: unset;
          margin-right: 2rem;
        }
      }

      .icons {
        align-items: center;
        position: absolute;
        right: 5%;
        top: 0;
        bottom: 0;
        width: 100px;
        margin: 0;
      }
    }
  }
}
</style>