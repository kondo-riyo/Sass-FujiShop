<template>
  <div class="">
    <div v-if="!moniterCarts" class="not_cartTable">
      <div>
        ※現在カートに商品はありません。
      </div>
    </div>

    <div v-if="moniterCarts">
      <div v-if="moniterCarts.itemInfo.length === 0" class="not_cartTable">
        <div>
          ※現在カートに商品はありません
        </div>
        <button>
          <router-link to="/">商品を選ぶ</router-link>
        </button>
      </div>
      <div v-if="moniterCarts.itemInfo.length > 0" class="cartTable">
        <div class="cartTable__card">
          <div class="cartTable__card__div">
            <div class="cartTable__card__head">
              <div class="info">商品情報</div>
              <div class="price">
                価格<span class="price__zei">(税抜)</span
                ><span class="price__en">(円)</span>
              </div>
              <div class="num">個数</div>
              <div class="total">
                合計<span class="total__zei">(税抜)</span
                ><span class="total__en">(円)</span>
              </div>
              <div class="empty"></div>
            </div>
            <div class="cartTable__card__body">
              <div
                v-for="cartitem in moniterCarts.itemInfo"
                :key="cartitem.specialId"
                class="body"
              >
                <div class="body__main">
                  <img class="" :src="cartitem.itemImg" />
                  <div class="body__main__msg">
                      <p>
                        {{ cartitem.itemName }}
                      </p>
                    <div
                      data-testid="toppingSize"
                      v-for="topping in cartitem.toppings"
                      :key="topping.id"
                    >
                      + {{ topping.name }} ({{ toppingSize(topping.size) }})
                    </div>
                  </div>
                </div>
                <div class="body__price">
                  <div class="body__price__main">
                    {{ cartitem.itemPrice
                    }}<span>円</span>
                  </div>
                  <div
                    v-for="topping in cartitem.toppings"
                    :key="topping.id"
                  >
                    + {{ topping.price
                    }}<span>円</span>
                  </div>
                </div>
                <div class="body__num">
                  {{ cartitem.itemNum }}<span>個</span>
                </div>
                <div class="body__total">
                  {{ cartitem.totalPrice}}
                  <span>円</span>
                </div>
                <div class="body__delete">
                  <button
                    title="商品を削除"
                    @click="deleteCartItem(cartitem.specialId)"
                    data-testid="show_cartItems"
                  >
                    <img src="~/assets/img/trash.webp" />
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="cartTable__card__foot">
          <div class="total__price">
            合計金額
            <span>
              {{ allItemsPrice }}</span
            >円
          </div>
          <div class="foot__button">
            <button
              v-show="moniterCarts.itemInfo.length > 0"
            >
              <router-link to="/OrderInfo"> 注文に進む </router-link>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { CartStore } from '../../../store';
export default Vue.extend({
  props: {
    cartItemFromStore: {
      type: Object,
      required: true,
      default: () => ({
        itemInfo: [],
        status: 0,
      }),
    },
  },
  methods: {
    toppingSize(el: number): string {
      if (el === 1) {
        return '多';
      } else if (el === 2) {
        return '少';
      } else {
        return '';
      }
    },
    deleteCartItem(id: string): void {
      if (confirm('カートから商品を削除しますか？')) {
        CartStore.deleteCartItemAct(id);
      }
    },
  },
  computed: {
    allItemsPrice(): number {
      let price = 0;
      // @ts-ignore
      this.cartItemFromStore.itemInfo.forEach((item) => {
        price = price + item.totalPrice;
      });
      return price;
    },
    moniterCarts(): any {
      // @ts-ignore
      return this.cartItemFromStore;
    },
  },
});
</script>
<style lang="scss">
@import "../../../style/templates/cart/cartTable.scss";

</style>