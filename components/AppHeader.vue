<template>
  <header class="header">
    <div class="container between">
      <div class="header__left">
        <nuxt-link to="/" class="logo">仔影视</nuxt-link>
        <nav class="hidden-sm-and-down">
          <ul>
            <li :class="route.path === '/' ? 'active' : ''">
              <NuxtLink to="/">首页</NuxtLink>
            </li>
            <li v-for="item in navigation.data.slice(0, 4)" :key="item.id">
              <nuxt-link
                :to="`/column/${item.value}`"
                v-if="+item.type === 1"
                >{{ item.name }}</nuxt-link
              >
              <nuxt-link
                :to="item.value"
                target="_blank"
                v-if="+item.type === 2"
                >{{ item.name }}</nuxt-link
              >
            </li>
          </ul>
        </nav>
      </div>
      <div class="header__right items-center">
        <el-input
          class="w-50 m-2 mr-20"
          placeholder="请输入搜索的影视名"
          :suffix-icon="ElIconSearch"
          v-model="searchValue"
          @keyup.enter.native="handleSearch"
        />
        <ClientOnly>
          <template v-if="token">
            <el-dropdown @command="handleCommand">
              <el-button
                circle
                :icon="ElIconUserFilled"
                color="#155FAA"
              ></el-button>
              <template #dropdown>
                <el-dropdown-menu>
                  <el-dropdown-item command="user">个人中心</el-dropdown-item>
                  <el-dropdown-item divided command="logOut"
                    >退出</el-dropdown-item
                  >
                </el-dropdown-menu>
              </template>
            </el-dropdown>
          </template>
          <template v-else>
            <el-button
              circle
              :icon="ElIconUserFilled"
              @click="goLogin"
            ></el-button>
          </template>
        </ClientOnly>
      </div>
    </div>
    <div class="mobile-nav hidden-sm-only hidden-sm-and-up">
      <ul>
        <li v-for="item in navigation.data" :key="item.id">
          <nuxt-link :to="`/column/${item.value}`" v-if="+item.type === 1">{{
            item.name
          }}</nuxt-link>
          <nuxt-link :to="item.value" target="_blank" v-if="+item.type === 2">{{
            item.name
          }}</nuxt-link>
        </li>
      </ul>
    </div>
  </header>
  <div class="header__height__placeholder"></div>
</template>
  
<script setup lang="ts">
import { useServerRequest } from "~/composables/useServerRequest";

const route = useRoute();

const searchValue = ref<string>("");
const token = ref<string | undefined>(undefined);
const { data: navigation, refresh } = await useServerRequest<{ data: any }>(
  "column/all",
  {
    query: { status: 0 },
  }
);

function handleCommand(command: string) {
  switch (command) {
    case "logOut":
      logOut();
      break;
    default:
      navigateTo("/user");
      break;
  }
}

function logOut() {}

function goLogin() {}

function handleSearch() {
  navigateTo("/search?keyword=" + searchValue.value);
}
</script>
  
  
  <style lang="scss">
.header {
  position: fixed;
  top: 0;
  z-index: 999;
  width: 100%;
  height: 55px;
  background-color: #292838;

  &__left {
    display: flex;
    .logo {
      display: flex;
      width: 150px;
      height: 55px;
      line-height: 55px;
      font-size: 24px;
      color: #ff9900;
      font-weight: bold;
      background-position: 50% 50% !important;
      background-size: cover !important;
      overflow: hidden;
    }
    nav {
      ul {
        display: flex;
        li {
          a {
            display: inline-block;
            height: 55px;
            line-height: 55px;
            font-size: 15px;
            padding: 0 20px;
            color: #fff;
          }
          &.active {
            a {
              background-color: #155faa;
              color: #fff;
            }
          }
        }
      }
    }
  }
  &__height__placeholder {
    height: 55px;
  }
}

@media only screen and (max-width: 991px) {
  .header {
    position: relative;

    .mobile-nav {
      border-top: #666 solid 1px;
      background: #292838;
      position: absolute;
      height: 46px;
      bottom: -46px;
      width: 100%;
      overflow-x: auto;
      overflow-y: hidden;
      box-sizing: border-box;
      padding-top: 8px;
      z-index: 9;
      &::-webkit-scrollbar {
        display: none;
      }
      ul {
        white-space: nowrap;
        li {
          display: inline-block;
          position: relative;
          &.active {
            a {
              color: #1583f3;
            }
          }
          a {
            display: inline-block;
            padding: 5px 19px;
            color: #ffffff;
            font-size: 16px;
          }
        }
      }
    }
  }
  .header__height__placeholder {
    height: 46px;
  }
}
</style>