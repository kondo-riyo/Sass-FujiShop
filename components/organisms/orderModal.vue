<template>
  <div>
    <div id="overlay" @click.self="$emit('close')">
      <div id="content" class="order_modal">
        <p
          class="order_modal__cross"
          @click="$emit('close')"
        >
          ×
        </p>
        <div class="order_modal__msg">
          <div>
            <div class="title">注文者</div>
            <div class="id_data">{{ orderInfo.name }} さん</div>
          </div>
          <div class="order_modal__msg">
            <div class="title">お届け先</div>
            <div class="id_data">{{ orderInfo.address }}</div>
          </div>
          <div class="order_modal__msg">
            <div class="title">電話番号</div>
            <div class="id_data">{{ orderInfo.tel }}</div>
          </div>
          <div class="order_modal__msg">
            <div class="title">メール</div>
            <div class="id_data">{{ orderInfo.email }}</div>
          </div>
          <div class="order_modal__msg">
            <div class="title">配達日時</div>
            <div class="id_data">
              {{ orderInfo.deliveryDate }} {{ orderInfo.deliveryTime }}時
            </div>
          </div>
          <div class="order_modal__msg">
            <div class="title">支払状況</div>
            <div data-testid="paymentStatus" class="id_data">
              {{ status | paymentStatus }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import Vue from 'vue';

type DataType = {
  showContent: boolean;
};
export default Vue.extend({
  props: ['orderInfo', 'status'],
  data(): DataType {
    return {
      showContent: false,
    };
  },
  filters: {
    paymentStatus(
      status: number
    ): '未入金' | '入金済' | '発送済' | '配達済' | 'キャンセル' | '確認中' {
      if (status === 1) {
        return '未入金';
      } else if (status === 2) {
        return '入金済';
      } else if (status === 3) {
        return '発送済';
      } else if (status === 4) {
        return '配達済';
      } else if (status === 9) {
        return 'キャンセル';
      } else {
        return '確認中'
      }
    },
  },
});
</script>

<style lang="scss">
  @import "../../style/organisms/orderModal.scss";

#overlay {
  /*要素を重ねた時の順番*/
  z-index: 1;

  /*画面全体を覆う設定*/
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(255, 255, 255, 0.042);

  /*画面の中央に要素を表示させる設定*/
  display: flex;
  align-items: center;
  justify-content: center;
}

#content {
  z-index: 2;
  /* width: 50%; */
  padding: 1em;
  background: rgba(248, 248, 248, 0.2);
  border-radius: 30px;
  border-color: rgba(153, 134, 117, 0.26);
  border-width: 4px;
  box-shadow: 20px;
}
</style>
