<template>
  <!-- <div class="w-screen"> -->
  <div class="default">
        <Header
      class="default__header"
    />
        <Nuxt
      class="default__nuxt"
    />
  </div>
</template>
<script lang="ts">
import Vue from 'vue';
import {
  ItemsStore,
  ToppingsStore,
  UserStore,
  CartStore,
  ApiItemsStore,
} from '../store';

export default Vue.extend({
  async fetch(): Promise<void> {
    // dbから商品とトッピングのデータを取得
    if (ItemsStore.items.length === 0 && ToppingsStore.toppings.length === 0) {
      const fetchItemsFromStore = ItemsStore.fetchItemsAct();
      const fetchToppingsFromStore = ToppingsStore.fetchToppingsAct();
      await Promise.all([fetchItemsFromStore, fetchToppingsFromStore]);
    }
    if (ApiItemsStore.itemsFromApi.length === 0) {
      const fetchApiItemsFromStore = ApiItemsStore.fetchApiItemsAct();
      await Promise.all([fetchApiItemsFromStore]);
    }
    if (UserStore.userInfo) {
      const fetchSignupInfo = UserStore.fetchSignupInfoAct();
      const fetchitemInfo = CartStore.fetchitemInfoAct();
      await Promise.all([fetchSignupInfo, fetchitemInfo]);
    }
  },
});
</script>
<style lang="scss">
.default {
  width: 100vw;
}
.default__header {
  width: 100vw;
}
.default__nuxt {
    width: 100vw;
  	min-height: 100vh;
}
</style>