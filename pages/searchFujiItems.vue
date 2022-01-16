<template>
  <div class="">
    <div>
      <div class="searchItems__title">
      <div><img src="~/assets/img/fujicoffee.webp" class="searchItems__title__icon"/></div>
      <div class="searchItems__title__msg">FujiCoffeeオリジナル商品</div>
      </div>
    </div>
    <search-group data-testid="search" @searchItems="search" class="searchItems__input" />
    <div class="searchItems__msg">
      <div v-show="resultNullFlg">
        ※ 該当する商品がありません
      </div>
      <div v-show="keywordNullFlg">
        ※ 検索ワードを入力してください
      </div>
    </div>

    <div class="searchItems__card">
      <div v-for="item in searchedItems" :key="item.id">
        <Card :item="item" :routerName="routerName.fujiShop" />
      </div>
    </div>

    <div
      class="searchItems__card"
      v-show="searchedItems.length === 0"
    >
      <div v-for="item in itemsFromStore" :key="item.id">
        <Card :item="item" :routerName="routerName.fujiShop" />
      </div>
    </div>
     <page-top-button />
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { itemType } from '../types/itemType';
import { ItemsStore } from '../store';
import Card from '../components/organisms/card.vue';
import searchGroup from '../components/morecules/searchGroup.vue';
import pageTopButton from '../components/atoms/button/pageTopButton.vue';
type routerName = {
  fujiShop: string
 }
type DataType = {
  searchedItems: itemType[];
  keywordNullFlg: boolean;
  resultNullFlg: boolean;
  routerName: routerName;
};
type headType = {
  title: string;
};

export default Vue.extend({
  head(): headType {
    return {
      title: 'FUJIオリジナル商品',
    };
  },
  components: { Card,searchGroup, pageTopButton },
  data(): DataType {
    return {
      searchedItems: [],
      keywordNullFlg: false, // 検索ワード未入力の場合
      resultNullFlg: false, // 該当結果無しの場合
      routerName: {
        fujiShop: 'ItemDetail-itemId',
      },
    };
  },
  // mounted(){
  //   window.addEventListener('scroll',this.handleScroll);
  // },
  methods: {
    search(word: string): {keywordNullFlg: boolean; resultNullFlg:boolean;}|undefined {
      this.searchedItems = [];
      // 検索ワードなし
      if (word === '') {
        return {
          keywordNullFlg: true,
          resultNullFlg: false
        }
      } else {
        this.keywordNullFlg = false;
        this.resultNullFlg = false;
        this.itemsFromStore.forEach((item: itemType) => {
          if (item.name === undefined) return;

          // 該当結果あり
          if (item.name.indexOf(word) >= 0) {
            this.searchedItems.push(item);
          }
        });
        // 該当結果なし
        if (this.searchedItems.length === 0) {
          this.resultNullFlg = true;
        }
      }
    },
  },
  computed: {
    itemsFromStore(): itemType[] {
      return ItemsStore.getItems;
    },
  },
});
</script>
<style lang="scss">
@import "../style/pages/searchItems.scss";

</style>