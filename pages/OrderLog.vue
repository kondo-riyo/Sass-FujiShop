<template>
  <div class="orderlog__base">
    <div v-if="getOrderLogs.length === 0" class="orderlog__not">
      <div>
        ※ 注文履歴はありません
      </div>
      <button>
        <router-link to="/">商品を選ぶ</router-link>
      </button>
    </div>
    <div
      class="orderlog"
      v-if="getOrderLogs.length > 0"
    >
      <div class="orderlog__table">
        <div class="orderlog__title">
          <div class="orderlog__title__item">
            商品情報
          </div>
          <div class="orderlog__title__deli">
            配達情報
          </div>
        </div>
        <div
          v-for="logItem in getOrderLogs"
          :key="logItem.orderId"
          class="orderlog__body"
        >
          <div class="orderlog__body__item">
            <div
              v-for="item in logItem.itemInfo"
              :key="item.specialId"
            >
              <!-- アイテム情報 -->
              <div class="orderlog__body__item_info">
                <div class="orderlog__body__item_img">
                  <img :src="item.itemImg" />
                </div>
                <div class="orderlog__body__item_msg">
                  <div class="orderlog__body__item_name">
                    <div class="orderlog__body__item_name__name">
                      <span>{{
                        item.itemName
                      }}</span
                      >×{{ item.itemNum }}
                    </div>
                    <div class="orderlog__body__item_name__price">
                      {{ item.itemPrice * item.itemNum }}円
                    </div>
                  </div>
                  <div
                    class="orderlog__body__item_topping"
                    v-for="(topping, index) in item.toppings"
                    :key="index"
                  >
                    <div class="orderlog__body__item_topping_name">
                      +{{ topping.name }}
                    </div>
                    <div class="orderlog__body__item_topping_price">
                      {{ topping.price }}円
                    </div>
                  </div>
                  <div class="orderlog__body__item_total">
                    <div class="orderlog__body__item_total_msg">合計</div>
                    <div class="orderlog__body__item_total_price">
                      {{
                        item.itemPrice * item.itemNum + item.allToppingPrice
                      }}円
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="orderlog__total">
              <div class="orderlog__total__msg">
                合計金額
              </div>
              <div class="orderlog__total__price">
                {{ logItem.orderInfo.allPrice }}円
              </div>
            </div>
          </div>
          <div class="orderlog__deli">
            <square-bottun 
             class="orderlog__deli__delibtn" 
             @click="openModal(logItem)"
            >
              配送情報確認
            </square-bottun>
            <div>
              <order-modal
                :status="mordalStatus"
                :orderInfo="mordalOrderInfo"
                v-show="showContent"
                @close="closeModal"
              ></order-modal>

              <div class="orderlog__deli__cancel">
                <square-bottun
                  class="orderlog__deli__cancel_btn"
                  v-show="logItem.status === 1"
                  @click="cancelOrder(logItem)"
                  data-testid="cancelOrder"
                  >注文キャンセル</square-bottun
                >
                <disableButton 
                 class="orderlog__deli__cancel_dis" 
                 v-show="logItem.status === 9"
                >
                キャンセル済み
                </disableButton>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
     <page-top-button />
  </div>
</template>
<script lang="ts">
import Vue from 'vue';
import { CartStore } from '../store';
import { orderedItemType } from '../types/cartItemType';
import disableButton from '../components/atoms/button/disableButton.vue';
import squareBottun from '../components/atoms/button/squareBottun.vue';
import orderModal from '../components/organisms/orderModal.vue' 
import pageTopButton from '../components/atoms/button/pageTopButton.vue';
type DataType = {
      showContent: boolean,
      mordalOrderInfo: string,
      mordalStatus: number,
}
type headType = {
  title: string;
};

export default Vue.extend({
  head(): headType {
    return {
      title: '注文履歴',
    };
  },
  components:{disableButton,squareBottun,orderModal, pageTopButton},
  data(): DataType {
    return {
      showContent: false,
      mordalOrderInfo: '',
      mordalStatus: 1,
    };
  },
  computed: {
    getOrderLogs(): orderedItemType[] {
      return CartStore.getOrderLog;
    },
  },
  async fetch(): Promise<void> {
      await CartStore.fetchOrderLogAct();
   },
  methods: {
    cancelOrder(logItem: orderedItemType): void {
      CartStore.cancelOrderAct(logItem);
    },
    openModal(logItem: any): void {
      this.showContent = true;
      this.mordalOrderInfo = logItem.orderInfo;
      this.mordalStatus = logItem.status;
    },
    closeModal(): void {
      this.showContent = false;
    },
  },
});
</script>
<style lang="scss">
@import "../style/pages/orderLog.scss";

</style>