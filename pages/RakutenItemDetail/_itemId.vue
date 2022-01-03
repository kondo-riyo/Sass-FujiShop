<template>
  <!-- <div class="sm:pl-10 p-1 flex"> -->
  <div class="itemId__base">
    <div v-if="!itemDetail">リロードして再度ご確認ください</div>
    <div class="itemId__base__center" v-if="itemDetail">
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

      <!-- <div class="sm:mt-5 mt-2 bg-white p-3 rounded-xl">
        <span class="font-bold"> {{ itemDetail.shopName }}</span>
        <p class="mb-3 pl-2 text-xl">
          {{ itemDetail.moreDescription }}
        </p>
      </div> -->

      <div class="raku_itemId__detail__msg">
        <span> {{ itemDetail.shopName }}</span>
        <p>
          {{ itemDetail.moreDescription }}
        </p>
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
import { ApiItemsStore, UserStore, CartStore } from '../../store';
import { itemType } from '../../types/itemType';
import { userInfoType } from '../../types/userInfoType';
import { cartItemType } from '../../types/cartItemType';
import Detail from '../../components/organisms/detail.vue';
import squareBottun from '../../components/atoms/button/squareBottun.vue';

type DataType = {
  itemDetail: itemType | undefined;
  itemNum: number[];
  selectedItemNum: number;
  params: string;
  userInfo: userInfoType | null;
};
type headType = {
  title: string;
};

export default Vue.extend({
  components: {
    Detail,
    squareBottun,
  },
  head(): headType {
    return {
      title: '商品詳細',
    };
  },
  data(): DataType {
    return {
      itemDetail: {},
      itemNum: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      selectedItemNum: 1,
      params: '',
      userInfo: UserStore.userInfo,
    };
  },
  async asyncData({ params }) {
    const paramsId = params.itemId;
    return { params: paramsId };
  },
  created(): void {
    this.getItemDetail();
  },
  methods: {
    cartAllPrice(): number {
      if (
        this.itemDetail === undefined ||
        this.itemDetail.price === undefined
      ) {
        return 0;
      }
      return this.selectedItemNum * this.itemDetail.price;
    },
    getItemDetail(): void {
      const getItemDetail: itemType | undefined = ApiItemsStore.getItemDetail(
        this.params
      );
      this.itemDetail = getItemDetail;
    },
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
          toppings: [],
          allToppingPrice: 0,
          totalPrice: this.calcTotalPrice,
        };
        if (confirm('カートに商品を追加しますか？')) {
          CartStore.addItemToCartAct(addItemToCart);
          // await this.$router.push('/Cart');
        }
      }
    },
    back_onStep(): void {
      this.$router.push('/searchRakutenItems');
    },
  },
  computed: {
    calcTotalPrice(): number {
      return this.cartAllPrice();
    },
  },
});
</script>
<style lang="scss">
@import "../../style/pages/itemId.scss";

</style>