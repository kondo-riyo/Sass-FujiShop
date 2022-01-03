<template>
  <div class="itemId__base">
    <div class="itemId__base__center">
      <!-- 戻るボタン追加 -->
    <div 
    @click="back_onStep"
    class="back_onStep">
      <div class="back_onStep__child">
        <img src="~assets/img/yajirusi_icon.webp">
      </div>
      <div class="back_onStep__msg">
        一覧に戻る
      </div>
    </div>

      <Detail
        :itemDetail="itemDetail"
        :options="itemNum"
        v-model="selectedItemNum"
      />

      <div class="itemId__topping">
        <p class="itemId__topping__title">
          トッピング<span> 少:200円 多:300円</span>
        </p>
        <div class="itemId__topping__card">
          <div
            class="itemId__topping__card__base"
            v-for="(topping, index) in getToppings"
            :key="topping.id"
          >
            <div class="topping__card__msg ">
              <p>{{ topping.name }}</p>
              <input
                data-testid="cal-modalM"
                type="radio"
                :name="'radio' + index"
                class="margin-left__1"
                @change="
                  selectToppingSize(
                    topping.name,
                    topping.id,
                    topping.priceM,
                    1,
                  )
                "
              />
              <span>少</span>
              <input
                data-testid="cal-modalL"
                type="radio"
                :name="'radio' + index"
                class="margin-left__1"
                @change="
                  selectToppingSize(
                    topping.name,
                    topping.id,
                    topping.priceL,
                    2,
                  )
                "
              />
              <span>多</span>
              <input
                data-testid="cal-modalNone"
                type="radio"
                :checked="true"
                :name="'radio' + index"
                class="margin-left__1"
                @change="
                  selectToppingSize(topping.name, topping.id, 0, 0),
                    clearTopping(topping.id)
                "
              />
              <span>なし</span>
            </div>
          </div>
        </div>
      </div>

      <div class="itemId__down">
        <div class="itemId__down__total">
          <p>
            合計
            <span>￥{{ calcTotalPrice }}</span>
          </p>
        </div>
        <div class="itemId__down__button">
          <div>
            <squareBottun
              data-testid="addCart"
              @click="addCart"
            >
              カートへ追加
            </squareBottun>
          </div>
          <div>
            <router-link to="/Cart">
              <squareBottun>
                カートを確認
              </squareBottun>
            </router-link>
          </div>
        </div>
      </div>

    <div 
    @click="back_onStep"
    class="back_onStep">
      <div class="back_onStep__child">
        <img src="~assets/img/yajirusi_icon.webp">
      </div>
      <div class="back_onStep__msg">
        一覧に戻る
      </div>
    </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { ItemsStore, ToppingsStore, CartStore, UserStore } from '../../store';
import { itemType } from '../../types/itemType';
import { toppingType } from '../../types/toppingType';
import { userInfoType } from '../../types/userInfoType';
import { cartItemType } from '../../types/cartItemType';
import squareBottun from '../../components/atoms/button/squareBottun.vue';
import Detail from '../../components/organisms/detail.vue';

type DataType = {
  itemDetail: itemType | undefined;
  itemNum: number[];
  selectedItemNum: number;
  allToppingPrice: number;
  selectedTopping: toppingType[];
  userInfo: userInfoType | null;
};
type headType = {
  title: string;
};

export default Vue.extend({
  head(): headType {
    return {
      title: '商品詳細',
    };
  },
  components: {
    squareBottun,
    Detail,
  },
  data(): DataType {
    return {
      itemDetail: {},
      itemNum: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      selectedItemNum: 1,
      allToppingPrice: 0,
      selectedTopping: [],
      userInfo: UserStore.userInfo,
    };
  },
  created(): void {
    const params: number = Number(this.$route.params.itemId);
    this.getItemDetail(params);
  },
  methods: {
    getItemDetail(params: number): void {
      const getItemDetail: itemType | undefined =
        ItemsStore.getItemDetail(params);
      this.itemDetail = getItemDetail;
    },
    // トッピング追加----------------------------------------------------
    selectToppingSize(
      selectedName: string|undefined,
      selecteId: number|undefined,
      toppingPrice: number|undefined,
      toppingSize: number|undefined,
    ): void {
      //同じトッピングを選択していないかチェック
      const duplicatedTopping = this.selectedTopping.findIndex(
        (topping) => topping.id === selecteId
      );
      // toppingの重複が無いとき
      if (duplicatedTopping < 0) {
        const pushTopping: toppingType = {
          name: selectedName,
          id: selecteId,
          price: toppingPrice,
          size: toppingSize,
        };
        this.selectedTopping.push(pushTopping);
      }
      //同じtoppingを選んだとき
      else if (duplicatedTopping >= 0) {
        // サイズを変更したとき
        if (this.selectedTopping[duplicatedTopping].size !== toppingSize) {
          this.selectedTopping[duplicatedTopping].size = toppingSize;
          this.selectedTopping[duplicatedTopping].price = toppingPrice;
        }
      }
      // topping価格の更新
      this.allToppingPrice = this.selectedTopping.reduce(
        (sum: number, addTopping: any) => sum + addTopping.price,
        0
      );
    },

    //トッピングを未選択に戻す
    clearTopping(selecteId: number|undefined): void {
      this.selectedTopping = this.selectedTopping.filter(
        (topping) => topping.size !== 0
      );
      const deleteToppigIndex = this.selectedTopping.findIndex(
        (topping) => topping.id === selecteId
      );
      this.selectedTopping.splice(deleteToppigIndex, 1);
      this.allToppingPrice = this.selectedTopping.reduce(
        (sum: number, addTopping: any) => sum + addTopping.price,
        0
      );
    },

    // カートに追加-------------------------------------------------------------------
    async addCart(): Promise<void> {
      if (!this.userInfo) {
        this.$router.push('/signin');
      } else {
        const addItemToCart: cartItemType = {
          itemId: this.itemDetail?.id,
          itemName: this.itemDetail?.name,
          itemPrice: this.itemDetail?.price,
          itemNum: this.selectedItemNum,
          itemImg: this.itemDetail?.img,
          toppings: this.selectedTopping,
          allToppingPrice: this.allToppingPrice,
          totalPrice: this.calcTotalPrice,
        };
        if (confirm('カートに商品を追加しますか？')) {
          CartStore.addItemToCartAct(addItemToCart);
          // await this.$router.push('/Cart');
        }
      }
    },
    back_onStep(): void {
      this.$router.push('/searchFujiItems');
    },
    cartAllPrice(): number {
      if (
        this.itemDetail === undefined ||
        this.itemDetail.price === undefined
      ) {
        return 0;
      } // 0:false
      return (
        this.selectedItemNum * this.itemDetail.price + this.allToppingPrice
      );
    },
  },
  computed: {
    getToppings(): toppingType[] {
      return ToppingsStore.getToppings;
    },
    calcTotalPrice(): number {
      return this.cartAllPrice();
    },
  },
});
</script>
<style lang="scss">
@import "../../style/pages/itemId.scss";

</style>