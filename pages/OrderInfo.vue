<template>
  <div class="orderinfo">
    <div class="orderinfo__base">
      <div class="orderinfo__title">
        <p>
          <img
            src="~/assets/img/coffeebeans_icon.webp"
          />
        </p>
        <p>お届け先情報</p>
        <p>
          <img
            src="~/assets/img/coffeebeans_icon.webp"
          />
        </p>
      </div>
      <div class="orderinfo__table">
        <div>
            <form
              @submit.prevent
            >
              <ValidationObserver v-slot="{ invalid }">
                <div class="orderinfo__table__base ">
                  <div class="orderinfo__table__name">
                    <label 
                      class=""
                      for="grid-first-name"
                    >
                      名前
                    </label>
                    <validation-provider
                      v-slot="{ errors }"
                      name="名前"
                      rules="required|max:10"
                    >
                      <inputA
                        data-testid="inputName"
                        v-model="name"
                        name="名前"
                        type="text"
                        placeholder="例)田中 太朗"
                        @input="inputName"
                      />
                      <span class="orderinfo__table__span">
                        {{ errors[0] }}
                      </span>
                    </validation-provider>
                  </div>
                  <div class="orderinfo__table__mail">
                    <label
                      for="grid-last-name"
                    >
                      メールアドレス
                    </label>
                    <validation-provider
                      v-slot="{ errors }"
                      name="メールアドレス"
                      rules="required|email"
                    >
                      <inputA
                        data-testid="inputMail"
                        v-model="email"
                        name="メールアドレス"
                        type="text"
                        placeholder="例)sample@gmail.com"
                        @input="inputMail"
                      />
                      <span class="orderinfo__table__span">
                        {{ errors[0] }}
                      </span>
                    </validation-provider>
                  </div>
                </div>
                <div class="orderinfo__table__base">
                  <div class="orderinfo__table__postal">
                    <label
                      for="grid-last-name"
                    >
                      郵便番号
                    </label>
                    <validation-provider
                      v-slot="{ errors }"
                      name="郵便番号"
                      rules="required|yubin"
                    >
                      <inputA
                        data-testid="inputPostalcode"
                        v-model="postalcode"
                        name="郵便番号"
                        type="text"
                        placeholder="例)1234567"
                        @input="inputPostalcode"
                      />
                      <span class="orderinfo__table__span">
                        {{ errors[0] }}
                      </span>
                    </validation-provider>
                  </div>
                  <div >
                    <square-bottun
                      data-testid="yubinbango"
                      @click="yubinbango()"
                      class="orderinfo__table__postal__btn"
                    >
                      検索
                    </square-bottun>
                  </div>
                  <div class="orderinfo__table__address">
                    <label
                      for="grid-last-name"
                    >
                      住所
                    </label>
                    <validation-provider
                      v-slot="{ errors }"
                      name="住所"
                      rules="required"
                    >
                      <inputA
                        data-testid="inputAddress"
                        v-model="address"
                        name="住所"
                        type="text"
                        placeholder="例)東京都新宿区〇〇"
                        @input="inputAddress"
                      />
                      <span class="orderinfo__table__span">
                        {{ errors[0] }}
                      </span>
                    </validation-provider>
                  </div>
                </div>
                <div class="orderinfo__table__base">
                  <div class="orderinfo__table__tel">
                    <label
                      for="grid-first-name"
                    >
                      電話番号
                    </label>
                    <validation-provider
                      v-slot="{ errors }"
                      name="電話番号"
                      rules="required|min:10|max:11"
                    >
                      <inputA
                        data-testid="inputTel"
                        v-model="tel"
                        name="電話番号"
                        type="text"
                        placeholder="例)00000000000"
                        @input="inputTel"
                      />
                      <span class="orderinfo__table__span">
                        {{ errors[0] }}
                      </span>
                    </validation-provider>
                  </div>
                  <div class="orderinfo__table__deli_date">
                    <label
                      for="grid-last-name"
                    >
                      配達日
                    </label>
                    <validation-provider
                      v-slot="{ errors }"
                      name="配達日"
                      rules="required"
                    >
                      <inputA
                        data-testid="inputDeliveryDate"
                        v-model="deliveryDate"
                        name="配達日"
                        type="date"
                        placeholder=""
                        @input="inputDeliveryDate"
                      />
                      <span class="orderinfo__table__span">
                        {{ errors[0] }}
                      </span>
                    </validation-provider>
                  </div>
                  <div class="orderinfo__table__deli_time">
                    <label
                      for="grid-last-name"
                    >
                      配達時間
                    </label>
                    <validation-provider
                      v-slot="{ errors }"
                      name="配達時間"
                      rules="required|delivary_validation:@配達日"
                    >
                      <delivery-select
                        v-model="deliveryTime"
                        name="配達時間"
                        :options="options"
                      ></delivery-select>
                      <span class="orderinfo__table__span">
                        {{ errors[0] }}
                      </span>
                    </validation-provider>
                  </div>
                </div>
                <div class="orderinfo__table__payment">
                  <div class="orderinfo__table__payment__radio">
                    <validation-provider
                      v-slot="{ errors }"
                      name="pay"
                      rules="oneOf:1,2"
                    >
                      <div>
                        <label>
                          <input
                            v-model="payment"
                            data-testid="notcreditPay"
                            type="radio"
                            name="代引き"
                            value="1"
                            @click="notcreditPay()"
                          />代金引換
                        </label>
                      </div>
                      <div>
                        <label>
                          <input
                            data-testid="creditPay"
                            v-model="payment"
                            type="radio"
                            name="クレジットカード払い"
                            value="2"
                            @click="creditPay()"
                          />クレジットカード
                        </label>
                      </div>
                      <span class="orderinfo__table__span">
                        {{ errors[0] }}
                      </span>
                    </validation-provider>
                  </div>
                  <div
                    class="orderinfo__table__credit"
                    v-show="selectPayment == true"
                    id="selectPayment"
                  >
                    <label
                      for="grid-last-name"
                    >
                      クレジットカード番号
                    </label>
                    <validation-provider
                      v-slot="{ errors }"
                      name="クレジットカード番号"
                      rules="required|creditNum"
                    >
                      <inputA
                        data-testid="inputCreditCardNum"
                        v-model="creditCardNum"
                        name="クレジットカード番号"
                        type="text"
                        placeholder="例)0000000000000000"
                        @input="inputCreditCardNum"
                      />
                      <span class="orderinfo__table__span">
                        {{ errors[0] }}
                      </span>
                    </validation-provider>
                  </div>
                </div>
                <div class="orderinfo__total__btn">
                  <div v-if="invalid">
                    ※入力内容が不足しています
                  </div>
                  <round-bottun
                    data-testid="OrderSubmit"
                    @click="OrderSubmit()"
                    :disabled="invalid"
                    v-if="!invalid"
                  >
                    ご注文を確定する
                  </round-bottun>
                </div>
              </ValidationObserver>
            </form>
          </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import Vue from 'vue';
import { ValidationProvider, ValidationObserver } from 'vee-validate';
import { UserStore, CartStore, AdminStore } from '../store';
import { orderItemType } from '../types/cartItemType';
import { userInfoType } from '../types/userInfoType';
import DeliverySelect from '../components/atoms/input/deliverySelect.vue';
import squareBottun from '../components/atoms/button/squareBottun.vue';
import roundBottun from '../components/atoms/button/roundBottun.vue';
import inputA from '../components/atoms/input/inputA.vue';
let YubinBango = require('yubinbango-core2');

type DataType = {
  name?: string;
  email?: string;
  postalcode?: string;
  address?: string;
  tel?: string;
  deliveryDate: string;
  deliveryTime: string;
  payment: number;
  creditCardNum: string;
  selectPayment: boolean;
  options: Array<{ [field: string]: any }>;
};

type headType = {
  title: string;
};

export default Vue.extend({
  data(): DataType {
    return {
      name: '',
      email: '',
      postalcode: '',
      address: '',
      tel: '',
      deliveryDate: '',
      deliveryTime: '',
      payment: 0,
      creditCardNum: '',
      selectPayment: false,
      options: [
        { label: '10時', value: '10' },
        { label: '11時', value: '11' },
        { label: '12時', value: '12' },
        { label: '13時', value: '13' },
        { label: '14時', value: '14' },
        { label: '15時', value: '15' },
        { label: '16時', value: '16' },
        { label: '17時', value: '17' },
      ],
    };
  },
  components: {
    ValidationProvider,
    ValidationObserver,
    DeliverySelect,
    squareBottun,
    roundBottun,
    inputA,
  },
  methods: {
    OrderSubmit(): void{
      // cartの商品のstatusを1又は２に変更。
      let orderInfo = {
        name: this.name,
        email: this.email,
        postalcode: this.postalcode,
        address: this.address,
        tel: this.tel,
        deliveryDate: this.deliveryDate,
        deliveryTime: this.deliveryTime,
        payment: Number(this.payment),
        creditCardNum: this.creditCardNum,
        selectPayment: this.selectPayment,
        allPrice: this.getAllPrice,
      };
      let orderInfoToDb: any;
      orderInfoToDb = { ...this.itemInfoFromStore[0], orderInfo: orderInfo };
      let adminToDb: any;
      adminToDb = {
        orderId: this.itemInfoFromStore[0].orderId,
        uid: this.userInfoFromStore!.uid,
        name: orderInfo.name,
      };
      if (confirm('注文を確定してもよろしいですか？')) {
        CartStore.updateOrderAct(orderInfoToDb);
        AdminStore.addAdminAct(adminToDb);
        this.$router.push('/OrderComp');
      }
    },
    creditPay(): void {
      this.selectPayment = true;
    },
    notcreditPay(): void {
      this.selectPayment = false;
    },
    yubinbango(): void {
      let newAddress = '';
      new YubinBango.Core(this.postalcode, (addr: any) => {
        newAddress = addr.region + addr.locality + addr.street;
        this.address = newAddress;
      });
    },
    inputName(value: string): void {
      this.name = value;
    },
    inputMail(value: string): void {
      this.email = value;
    },
    inputPostalcode(value: string): void {
      this.postalcode = value;
    },
    inputAddress(value: string): void {
      this.address = value;
    },
    inputTel(value: string): void {
      this.tel = value;
    },
    inputDeliveryDate(value: string): void {
      this.deliveryDate = value;
    },
    inputCreditCardNum(value: string): void {
      this.creditCardNum = value;
    },
  },
  //async fetchでcartの商品を取得する(cartStore)
  computed: {
    userInfoFromStore(): userInfoType | null {
      return UserStore.getUserInfo;
    },
    itemInfoFromStore(): orderItemType[] {
      return CartStore.getitemInfo;
    },
    getAllPrice(): number {
      let allPrice: number = 0;
      //@ts-ignore
      CartStore.getitemInfo[0].itemInfo!.forEach((item) => {
        allPrice = allPrice + item.totalPrice!;
      });
      return allPrice;
    },
  },
  mounted(): void {
    if(UserStore.getUserInfo){
      this.name = UserStore.getUserInfo!.name,
      this.email = UserStore.getUserInfo!.email,
      this.postalcode = UserStore.getUserInfo!.postalcode,
      this.address = UserStore.getUserInfo!.address,
      this.tel = UserStore.getUserInfo!.tel
    }
  },
  head(): headType {
    return {
      title: 'お届け先情報入力',
    };
  },
});
</script>
<style lang="scss">
@import "../style/pages/orderInfo.scss";

</style>