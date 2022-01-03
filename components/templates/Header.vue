<template>
  <nav class="header">
    <!-- mainLogoとuserName -->
    <div class="header__main__icon">
      <div class="">
        <router-link to="/" class="header__main__icon__router ">
          <img src="~/assets/img/6.5.webp" class="img"/>
          <span>FUJI SHOP</span>
        </router-link>
      </div>
    </div>

    <!-- menu -->
    <div class="header__menu">
      <div
        v-if="userInfoFromStore"
        class="header__menu__name"
      >
        {{ userInfoFromStore.name }}さん
      </div>

      <div class="header__menu__icon" v-if="userInfoFromStore">
        <router-link to="/cart">
        <!-- <span class="relative inline-block">
          <span class="absolute top-0 right-0 inline-flex items-center justify-center px-2 py-1 text-xs font-bold leading-none text-red-100 transform translate-x-1/2 -translate-y-1/3 bg-red-600 rounded-full">
            {{itemInfoFromStore[0].itemInfo.length}}
          </span>
        </span> -->
          <img src="~/assets/img/cart_icon.webp"
        />
        </router-link>
      </div>
      <div class="header__menu__icon">
        <router-link to="/orderLog" v-if="userInfoFromStore">
          <img src="~/assets/img/rireki.webp"
        /></router-link>
      </div>
      <div class="header__menu__icon">
        <router-link to="/admin" v-if="userInfoFromStore">
          <img src="~/assets/img/setting.webp"
        /></router-link>
      </div>
      <div class="header__menu__icon" data-testid="logout" v-show="userInfoFromStore" @click="logout">
        <img src="~/assets/img/logout_icon.webp" />
      </div>
      <div class="header__menu__icon" v-if="!userInfoFromStore">
        <router-link to="/signup">
          <img src="~/assets/img/user.webp"
        /></router-link>
      </div>

      <div class="header__menu__icon" v-if="!userInfoFromStore">
        <router-link to="/signin">
          <img src="~/assets/img/login_icon.webp"
        /></router-link>
      </div>
    </div>
  </nav>
</template>

<script lang="ts">
import { UserStore } from '../../store';
import Vue from 'vue';
import { userInfoType } from '../../types/userInfoType';

export default Vue.extend({
  methods: {
    logout(): void {
        UserStore.logout();
        this.$router.push('/');
    },
  },
  computed: {
    userInfoFromStore(): userInfoType {
      return UserStore.getUserInfo!;
    },
  },
});
</script>
<style lang="scss">
@import "../../style/templates/Header.scss";

</style>
