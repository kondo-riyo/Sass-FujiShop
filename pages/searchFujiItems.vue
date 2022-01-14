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
      <button class="page_top" @click="scrollTop">
        Topに戻る
      </button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { itemType } from '../types/itemType';
import { ItemsStore } from '../store';
import Card from '../components/organisms/card.vue';
import searchGroup from '../components/morecules/searchGroup.vue';
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
  components: { Card,searchGroup },
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
    scrollTop(){
      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });
    }
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
.page_top {
    position: fixed;
    right: 10px;
    bottom: 10px;
    font-size: 1.2rem;
    line-height: 1.2rem;
    background: #fff;
    color: #737373;
    padding: 10px;
    border: solid 1px;
    border-radius: 50%;
    box-shadow: 0 2px 10px -6px rgba(0,0,0,.5), 0 3px 10px -4px rgba(0,0,0,.2);
}

// p{
//   text-align: center;
// }
// .headerArea{
//  height:50vh;
//  width:100%;
//  background-color:#5E454B;
//  margin: 0;
//  text-align: center;
// }
// .contents{
//  height:120vh;
//  width:100%;
//  background-color:#F8F0DF;
//  margin: 0;
//  text-align: center;
// }
// h1,.hp{
//   color: #F3F0D7;
// }
// h2{
//  margin: 0;
// }
// #page_top{
//   width: 90px;
//   height: 90px;
//   position: fixed;
//   right: 10px;
//   bottom: 0;
//   opacity: 0;
//   transition: all 1s ease;
// }
// #page_top.show {
//     opacity: 0.6;
//     transform: none;
//  }
// #page_top a{
//   position: relative;
//   display: block;
//   width: 90px;
//   height: 90px;
//   text-decoration: none;
// }
// #page_top a::before{
//   font-family: 'Font Awesome 5 Free';
//   font-weight: 900;
//   content: '\f102';
//   font-size: 30px;
//   color: #A12568;
//   position: absolute;
//   width: 30px;
//   height: 30px;
//   top: -40px;
//   bottom: 0;
//   right: 0;
//   left: 0;
//   margin: auto;
//   text-align: center;
// }
// #page_top a::after{
//   content: 'TOP';
//   font-size: 18px;
//   position: absolute;
//   top: 45px;
//   bottom: 0;
//   right: 0;
//   left: 0;
//   margin: auto;
//   text-align: center;
//   color: #A12568;
// }
</style>