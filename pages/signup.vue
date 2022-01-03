<template>
  <div class="signup">
    <div class="signup__base">
      <div
        type="text"
        class="signup__table"
      >
        <div>
          <h1 class="signup__title">会員登録</h1>
        </div>
        <ValidationObserver v-slot="{ invalid }">
          <div class="signup__all">
            <div>
              <label>
                名前
              </label>
              <validation-provider
                v-slot="{ errors }"
                name="名前"
                rules="required|max:10"
              >
                <inputA
                  data-testid="inputName"
                  name="名前"
                  type="text"
                  placeholder="例)田中 太朗"
                  @input="inputName"
                />
                <span>
                  {{ errors[0] }}
                </span>
              </validation-provider>
            </div>
            <div>
              <label>
                メールアドレス
              </label>
              <validation-provider
                v-slot="{ errors }"
                name="メールアドレス"
                rules="required|email"
              >
                <inputA
                  data-testid="inputMail"
                  name="メールアドレス"
                  type="text"
                  placeholder="例)sample@gmail.com"
                  @input="inputMail"
                />
                <span>
                  {{ errors[0] }}
                </span>
              </validation-provider>
            </div>
            <div class="signup__postal">
              <div class="signup__postal_code">
                <label>
                  郵便番号
                </label>
                <validation-provider
                  v-slot="{ errors }"
                  name="郵便番号"
                  rules="required|yubin"
                >
                  <inputA
                    data-testid="inputPostalcode"
                    v-model="userInfo.postalcode"
                    name="郵便番号"
                    type="number"
                    placeholder=""
                    @input="inputPostalcode"
                  />
                  <span>
                    {{ errors[0] }}
                  </span>
                </validation-provider>
              </div>
              <div class="signup__postal__btn">
                <round-bottun
                  data-testid="yubinbango"
                  @click="yubinbango()"
                  class="signup__postal__btn_search"
                >
                  検索
                </round-bottun>
              </div>
            </div>
            <div>
              <label>
                住所
              </label>
              <validation-provider
                v-slot="{ errors }"
                name="住所"
                rules="required"
              >
                <inputA
                  data-testid="inputAddress"
                  v-model="userInfo.address"
                  name="住所"
                  type="text"
                  placeholder="東京都新宿区"
                  @input="inputAddress"
                />
                <span>
                  {{ errors[0] }}
                </span>
              </validation-provider>
            </div>
            <div>
              <label>
                電話番号
              </label>
              <validation-provider
                v-slot="{ errors }"
                name="電話番号"
                rules="required|min:10|max:11"
              >
                <inputA
                  data-testid="inputTel"
                  name="電話番号"
                  type="text"
                  placeholder="例)090XXXXXXXX"
                  @input="inputTel"
                />
                <span>
                  {{ errors[0] }}
                </span>
              </validation-provider>
            </div>
            <div>
              <label>
                パスワード
              </label>
              <validation-provider
                v-slot="{ errors }"
                name="パスワード"
                rules="required"
              >
                <div class="signup__pass">
                  <inputA
                    data-testid="inputPassword"
                    name="パスワード"
                    :type="inputType"
                    placeholder="例)*******"
                    @input="inputPassword"
                  />
                  <div @click="onClick" class="signup__pass__show" data-testid="onClick">
                    <div v-show="isChecked">
                      <img src="~/assets/img/eye_icon.webp" />
                    </div>
                    <div v-show="!isChecked">
                      <img src="~/assets/img/noeye_icon.webp" />
                    </div>
                  </div>
                </div>
                <span>
                  {{ errors[0] }}
                </span>
              </validation-provider>
            </div>
            <div class="signup__btn">
              <div v-if="invalid" class="">
                <div class="signup__btn__not">
                  ※入力内容が不足しています
                </div>
              </div>
              <div class="">
                <round-bottun
                  data-testid="signup"
                  @click="signup"
                  :disabled="invalid"
                  v-if="!invalid"
                  class="signup__btn__comp"
                >
                  登録
                </round-bottun>
              </div>
            </div>
          </div>
        </ValidationObserver>
        <div class="signup__send">
          <router-link to="/signin">
            会員登録済みの方はこちら
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import Vue from 'vue';
import { ValidationProvider, ValidationObserver } from 'vee-validate';
import firebase, { auth, db } from '../plugins/firebase';
import { userInfoType } from '../types/userInfoType';
import roundButton from '../components/atoms/button/roundBottun.vue';
import inputA from '../components/atoms/input/inputA.vue';

let YubinBango = require('yubinbango-core2');

type DataType = {
  userInfo: userInfoType;
  isChecked: boolean;
};
type headType = {
  title: string;
};
export default Vue.extend({
  components: {
    ValidationProvider,
    ValidationObserver,
    roundButton,
    inputA,
  },
  data(): DataType {
    return {
      userInfo: {
        name: '',
        email: '',
        password: '',
        tel: '',
        postalcode: '',
        address: '',
        uid: '',
      },
      isChecked: false,
    };
  },
  head(): headType {
    return {
      title: '会員登録',
    };
  },
  methods: {
    async signup(): Promise<void> {
      try {
        if (
          this.userInfo.email === undefined ||
          this.userInfo.password === undefined
        )
          return;
        const authUser: firebase.auth.UserCredential =
          await auth.createUserWithEmailAndPassword(
            this.userInfo.email,
            this.userInfo.password
          );
        if (authUser.user === null) return;
        await this.addAuthUserToDb(authUser.user.uid);
        this.$router.push('/signin');
      } catch (error) {
        // @ts-ignore
        console.log(error.message);
      }
    },
    addAuthUserToDb(
      uid: string
    ): Promise<
      firebase.firestore.DocumentReference<firebase.firestore.DocumentData>
    > {
      return db.collection(`users/${uid}/userInfo`).add({
        email: this.userInfo.email,
        name: this.userInfo.name,
        tel: this.userInfo.tel,
        postalcode: this.userInfo.postalcode,
        address: this.userInfo.address,
        uid: uid,
      });
    },
    inputName(value: string): void {
      this.userInfo.name = value;
    },
    inputMail(value: string): void {
      this.userInfo.email = value;
    },
    inputPassword(value: string): void {
      this.userInfo.password = value;
    },
    inputTel(value: string): void {
      this.userInfo.tel = value;
    },
    inputPostalcode(value: string): void {
      this.userInfo.postalcode = value;
    },
    inputAddress(value: string): void {
      this.userInfo.address = value;
    },
    yubinbango(): void {
      console.log(this.userInfo.postalcode);
      let newAddress = '';
      new YubinBango.Core(this.userInfo.postalcode, (addr: any) => {
        newAddress = addr.region + addr.locality + addr.street;
        this.userInfo.address = newAddress;
      });
    },
    onClick(): void {
      this.isChecked = !this.isChecked;
    },
  },
  computed: {
    inputType(): string {
      return this.isChecked ? 'text' : 'password';
    },
  },
});
</script>
<style lang="scss">
@import "../style/main.scss";
@import "../style/pages/signup.scss";
.signup {
  background-color: $base_gray--25;
  display: flex;
  justify-content: center;
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  @media (min-width: 640px) {
    background-image: url("../assets/img/coffee-2734137_1280.webp");
  }
}

</style>