<template>
  <div class="ordercomp">
    <div class="ordercomp__base">
      <img src="~/assets/img/ordercomp.webp" />
      <div class="ordercomp__msg">
        <div v-if="userInfoFromStore" class="ordercomp__msg__name">
          {{ userInfoFromStore.name }} 様
        </div>
        <div class="ordercomp__msg__thank">
          この度はご購入いただきありがとうございました
        </div>
        <p class="ordercomp__msg__check">
          ご購入いただいた商品は、<br />注文履歴よりご確認いただけます。<br />
        </p>
        <p class="ordercomp__msg__yaji">
          ↓
        </p>
      </div>
      <div>
        <round-bottun>
          <router-link to="OrderLog">
            注文内容を確認する
          </router-link>
        </round-bottun>
      </div>
      <div class="ordercomp__button">
        <round-bottun>
          <router-link to="/">
          ホームに戻る
          </router-link>
        </round-bottun>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import Vue from 'vue';
import { UserStore } from '../store';
import { userInfoType } from '../types/userInfoType';
import roundBottun from '../components/atoms/button/roundBottun.vue'

type headType = {
  title: string;
};

export default Vue.extend({
  components:{
    roundBottun
  },
  computed: {
    userInfoFromStore(): userInfoType | null {
      return UserStore.getUserInfo;
    },
  },
  head(): headType {
    return {
      title: '注文完了',
    };
  },
});
</script>
<style lang="scss">
@import "../style/pages/orderComp.scss";

</style>