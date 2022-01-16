<template>
  <div>
      <!-- <div 
       class="
        flex 
        w-full justify-center items-end 
        text-base_brown mt-4 -mb-2 text-center text-xl sm:text-3xl font-bold
      "> -->
      <div class="searchItems__title">
      <div>
        <!-- <img src="~/assets/img/Rakuten.webp" class="w-8 sm:w-10"/> -->
        <img src="~/assets/img/Rakuten.webp" class="searchItems__title__icon"/>
      </div>
      <div class="">楽天商品</div>
      </div>

    <search-group data-testid="search" @searchItems="search" class="searchItems__input" />

    <!-- 検索エラー表示 -->
    <div class="searchItems__msg">
      <p v-show="keywordNullFlg">検索ワードを入力して下さい</p>
      <p v-show="searchItemNullFlg">検索ワードにマッチする商品がありません</p>
    </div>

    <!-- 検索結果 -->
    <div v-show="!defaultItemsFlg" class="searchItems__card">
      <div v-for="item in searchApiItemsFromStore" :key="item.id">
        <Card :item="item" :routerName="routerName.rakuten" />
      </div>
    </div>

    <!-- top30 -->
    <div
      v-show="defaultItemsFlg || keywordNullFlg || searchItemNullFlg"
      class="searchItems__card"
    >
      <div v-for="item in apiItemsFromStore" :key="item.id">
        <Card :item="item" :routerName="routerName.rakuten" />
      </div>
    </div>
     <page-top-button />
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { ApiItemsStore } from '../store';
import { itemType } from '../types/itemType';
import Card from '../components/organisms/card.vue';
import searchGroup from '../components/morecules/searchGroup.vue';
import pageTopButton from '../components/atoms/button/pageTopButton.vue';
type routerName = {
 rakuten: string;
}
type DataType = {
  defaultItemsFlg: boolean;
  keywordNullFlg: boolean;
  searchItemNullFlg: boolean;
  routerName: routerName;
};

type headType = {
  title: string;
};

export default Vue.extend({
  head(): headType {
    return {
      title: '楽天商品',
    };
  },
  components: { Card, searchGroup, pageTopButton },
  data(): DataType {
    return {
      defaultItemsFlg: true,
      keywordNullFlg: false,
      searchItemNullFlg: false,
      routerName: {
        rakuten: 'RakutenItemDetail-itemId',
      },
    };
  },
  created(): void{
    // 遷移したらtop30が表示される
    this.defaultItemsFlg = true;
  },
  computed: {
    apiItemsFromStore(): itemType[] {
      return ApiItemsStore.getItemsFromApi;
    },
    searchApiItemsFromStore(): itemType[] {
      return ApiItemsStore.getSearchItemsFromApi;
    },
  },
  methods: {
    async search(word: any): Promise<void> {
      if (word !== '') {
        this.keywordNullFlg = false;
        await ApiItemsStore.searchApiItemsAct(word);
        if (this.searchApiItemsFromStore.length > 0) {
          this.searchItemNullFlg = false;
          this.defaultItemsFlg = false;
        } else {
          this.searchItemNullFlg = true;
          this.defaultItemsFlg = true;
        }
      } else {
        this.defaultItemsFlg = true;
        this.keywordNullFlg = true;
        this.searchItemNullFlg = false;
      }
    },
  },
});
</script>
<style lang="scss">
@import "../style/pages/searchItems.scss";

</style>