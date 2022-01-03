<template>
   <div class="adminId__base">
    <div 
    @click="back_onStep"
    class="back_onStep">
      <div class="back_onStep__child">
        <img src="~assets/img/yajirusi_icon.webp">
      </div>
      <div class="back_onStep__msg">
        ユーザー情報一覧に戻る
      </div>
    </div>

    <div class="adminId">
      <div class="adminId__table">
        <div
          class="adminId__table__head"
        >
          <div class="adminId__table__head__proinfo">
            商品情報
          </div>
          <div class="adminId__table__head__deliinfo">
            配達情報
          </div>
          <div class="adminId__table__head__em">
          </div>
        </div>
        <div
          v-for="logItem in getLogItems"
          :key="logItem.orderId"
          class="adminId__table__body"
        > 
        <!--   1. アイテム情報 -->
          <div 
           class="adminId__item_info" >
            <div
              v-for="item in logItem.itemInfo"
              :key="item.specialId"
            >
              <div class="adminId__item_info__up">
                <div class="item_info__up__img">
                  <img :src="item.itemImg" />
                </div>
                <div class="item_info__up__msg">
                  <div class="flex">
                    <div class="item_info__up__msg__itemName">
                      <span>{{
                        item.itemName
                      }}</span
                      >×{{ item.itemNum }}
                    </div>
                    <div class="item_info__up__msg__itemPrice">
                      {{ item.itemPrice * item.itemNum }}円
                    </div>
                  </div>
                  <div
                    class="item_info__up__msg__topping"
                    v-for="(topping, index) in item.toppings"
                    :key="index"
                  >
                    <div class="item_info__up__msg__topping__name">+{{ topping.name }}</div>
                    <div class="item_info__up__msg__topping__price">
                      {{ topping.price }}円
                    </div>
                  </div>
                  <div class="item_info__up__msg__total">
                    <div class="item_info__up__msg__total__gokei">合計</div>
                    <div class="item_info__up__msg__total__price">
                      {{
                        item.itemPrice * item.itemNum + item.allToppingPrice
                      }}円
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="adminId__item_info__down">
              <div class="item_info__down__em"></div>
              <div class="item_info__down__msg">
                合計金額
              </div>
              <div class="item_info__down__price">
                {{ logItem.orderInfo.allPrice }}円
              </div>
            </div>
         </div>
        <!-- 1. お客様情報 -->
         <div class="adminId__user_info">
            <div>
              <div class="user_info__title">
                配達情報
              </div>
              <div>注文者 : {{ logItem.orderInfo.name}} 様</div>
              <div>配送先 : {{ logItem.orderInfo.address }}</div>
              <div>
                配送日時 : {{ logItem.orderInfo.deliveryDate }}
                {{ logItem.orderInfo.deliveryTime }}時
              </div>
            </div>
          </div>
          <!-- 1. 配達情報 -->
          <div class="adminId__deli_info">
            <div>
              <select
                v-model="logItem.status"
                @change="statusChange(logItem.orderId, logItem.status, params)"
                name="配達status"
                data-testid="statusChange"
              >
                <option value=1>未入金(代引き)</option>
                <option value=2>入金済</option>
                <option value=3>発送済</option>
                <option value=4>配達済</option>
                <option value=9>キャンセル</option>
              </select>
            </div>
          </div>
        </div>
        </div>

          <!-- 1. 終わり -->

    </div>
    <div 
    @click="back_onStep"
    class="back_onStep">
      <div class="back_onStep__child">
        <img src="~assets/img/yajirusi_icon.webp">
      </div>
      <div class="back_onStep__msg">
        ユーザー情報一覧に戻る
      </div>
    </div>
   </div>
</template>
<script lang="ts">
import Vue from 'vue';
import { AdminStore } from '../../store';
import { orderedItemType } from '../../types/cartItemType';

type DataType = {
  selectStatus: number;
  statusName: string;
  newlogItems: any;
  params: string;
  options:[
    {label: string, value: number},
    {label: string, value: number},
    {label: string, value: number},
    {label: string, value: number},
    {label: string, value: number},
  ]
};
type headType = {
  title: string;
};

export default Vue.extend({
  head(): headType {
    return {
      title: '注文履歴',
    };
  },
  data(): DataType {
    return {
      selectStatus: 0,
      statusName: '',
      newlogItems: [],
      params: '',
      options:[
        {label: '未入金(代引き)', value: 1},
        {label: '入金済', value: 2},
        {label: '発送済', value: 3},
        {label: '配達済', value: 4},
        {label: 'キャンセル', value: 9}
      ]
    };
  },
  computed: {
    getLogItems(): orderedItemType[] {
    // getLogItems():void{
      return  AdminStore.getstoreLogItems
      //@ts-ignore
      // return this.displaySelect
    },
  },
  async fetch(): Promise<void> {
      this.params = String(this.$route.params.adminId);
      await AdminStore.fetchLogItemsAct(this.params);
  },  
  methods:{
      // statusを変えるためのStore呼び出し
      statusChange(id:string, status:number, uid:string):void{
          let idStatus= {id:id, status:Number(status), uid:uid}
          AdminStore.updateStatusAct(idStatus)
      },
      back_onStep(): void {
        this.$router.push('/admin')
      },
      selectToStatus(value: any) {
        this.selectStatus = value
        console.log('selectToStatus'+this.selectStatus)
      },
  }
});
</script>
<style lang="scss">
@import "../../style/pages/adminId.scss";

</style>