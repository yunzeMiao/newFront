<template>
  <header id="header" ref="header">
    <b-navbar
      toggleable="lg"
      type="dark"
      variant="dark"
      class="active2"
      :class="{ active: showNavbarBg }"
    >
      <div class="container">
        <!-- logo -->
        <b-navbar-brand to="/">
          <img
            src="@/assets/img/logo.png"
            class="mr-2"
            alt="logo"
            style="width: 50px"
          />
        </b-navbar-brand>
        <!-- 切换按钮 -->
        <b-navbar-toggle
          target="nav-collapse"
          :class="{ active: showCollaps }"
        ></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav v-model="showCollaps">
          <b-navbar-nav class="ml-auto">
            <template v-for="(item, index) of navBarList" :Key="index">
              <b-nav-item
                v-if="item.children.length <= 0"
                :to="item.link"
                :active="$route.path === item.link"
              >
                {{ item.nav }}
              </b-nav-item>
              <template v-else>
                <b-nav-item-dropdown @show="handleShowDrop" right>
                  <template v-slot:button-content>
                    <span
                      @click="handleNavbarClick(item, '', $event)"
                      :class="{ active: $route.path.includes(item.link) }"
                      >{{ item.nav }}</span
                    >
                  </template>
                  <!-- 子菜单 -->
                  <b-dropdown-item
                    v-for="(ite, ind) of item.children"
                    :key="ind"
                    @click="handleNavbarClick(ite, ind, $event)"
                    >{{ ite.nav }}
                  </b-dropdown-item>
                </b-nav-item-dropdown>
              </template>
            </template>
          </b-navbar-nav>
        </b-collapse>
      </div>
    </b-navbar>
  </header>
</template>

<script>
export default {
  data() {
    return {
      showCollaps: false,
      showNavbarBg: false,
      showDrop: false,
      navBarList: [
        { nav: "home", link: "/", children: [] },
        { nav: "previous", link: "/previous", children: [] },
        { nav: "share", link: "/share", children: [] },
        { nav: "about us", link: "/aboutus", children: [] },
      ],
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.getHeaderHeight();
    });
    window.addEventListener("scroll", this.handleWindowScroll);
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.handleWindowScroll);
  },
  methods: {
    // 导航栏点击事件
    handleNavbarClick(item, index, bvEvt) {
      if (index !== "") {
        this.$store.commit("setSubNavIndex", index + 1);
      } else {
        this.$store.commit("setSubNavIndex", 0);
        this.showDrop = false;
      }
      this.$router.push(item.link);
      this.showCollaps = false;
    },
    handleShowDrop(bvEvt) {
      if (!this.showDrop) {
        bvEvt.preventDefault();
      }
      this.showDrop = true;
    },
    // 监听窗口滚动，改变导航背景色
    handleWindowScroll(e) {
      const myTop =
        document.documentElement.scrollTop || document.body.scrollTop;
      const flag = myTop > 200;
      if (this.showNavbarBg != flag) {
        this.showNavbarBg = flag;
      }
    },
    // 获取导航栏高度
    getHeaderHeight() {
      const headerHeight = this.$refs.header.clientHeight;
      this.$store.commit("setHeaderHeight", headerHeight);
    },
  },
};
</script>

<style lang="scss" scoped>
@import "@/assets/styles/variables.scss";

#header {
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  z-index: 99;
}

::v-deep.dropdown:hover .btn,
::v-deep.dropdown-item:hover {
  border-color: transparent;
  background: none;
}
::v-deep.nav-link:hover,
::v-deep.dropdown-item:hover,
::v-deep .navbar-nav .nav-link.active {
  color: $theme-color !important;
}

::v-deep.navbar-toggler {
  padding: 0;
  border: none;
  outline: none;
  .navbar-toggler-icon {
    position: relative;
    width: 1em;
    // height: 0.1em;
    height: 2px;
    border-radius: 1px;
    background: #fff;
    &:after,
    &:before {
      position: absolute;
      left: 0;
      content: "";
      width: 1em;
      height: 2px;
      // height: 0.1rem;
      background: #fff;
      transition: all 0.2s ease-in-out;
    }
    &:before {
      top: -0.3em;
    }
    &:after {
      top: 0.3em;
    }
  }
  &.active .navbar-toggler-icon {
    background: transparent;
    &:before {
      transform: rotate(45deg);
      transform-origin: 8%;
    }
    &:after {
      transform: rotate(-45deg);
      transform-origin: 8%;
    }
  }
}
.dropdown-toggle .active {
  color: $theme-color;
}
::v-deep .nav-link {
  color: #fff !important;
}
::v-deep .navbar {
  // background: transparent !important;
  background-color: #333;
  transition: 0.3s;
  &.active {
    background: #fff !important;
    box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075);
    transform: translateY(-100%);
    transition: 0.3s ease-in;
    .navbar-brand,
    .nav-link {
      color: #333 !important;
      &.active {
        color: #3269a5 !important;
      }
    }
    .navbar-toggler {
      .navbar-toggler-icon {
        background: #333;
        &:after,
        &:before {
          background: #333;
        }
      }
      &.active .navbar-toggler-icon {
        background: transparent;
      }
    }
  }
}
@media screen and (max-width: 992px) {
  ::v-deep .navbar {
    background: #fff !important;
    box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075);
    &.active2 {
      .navbar-brand,
      .nav-link {
        color: #333 !important;
        &.active {
          color: #3269a5 !important;
        }
      }
      .navbar-toggler {
        .navbar-toggler-icon {
          background: #333;
          &:after,
          &:before {
            background: #333;
          }
        }
        &.active .navbar-toggler-icon {
          background: transparent;
        }
      }
    }
  }
}
@media screen and (min-width: 992px) {
  ::v-deep.dropdown:hover .dropdown-menu {
    display: block;
  }
  ::v-deep .dropdown-menu {
    display: none;
    margin-top: 20px;
    border-color: transparent;
    box-shadow: 0 3px 12px rgba(0, 0, 0, 0.05);
    &:after {
      content: "";
      position: absolute;
      top: -20px;
      left: 0;
      width: 100%;
      height: 20px;
      background: transparent;
    }
    &:before {
      content: "";
      position: absolute;
      top: -13px;
      right: 1px;
      width: 0;
      height: 0;
      border: 7px solid transparent;
      border-bottom-color: #fff;
    }
  }
}
</style>

