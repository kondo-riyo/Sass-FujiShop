<template>
  <div class="signin">
    <div class="">
      <div
        type="text"
        class="signin__base"
      >
        <div>
          <h1 class="signin__title">ログイン</h1>
        </div>
        <ValidationObserver v-slot="{ invalid }">
          <div>
            <label>
             メール
            </label>
            <validation-provider
              v-slot="{ errors }"
              name="メールアドレス"
              rules="required|email"
            >
              <inputA
                data-testid="inputMail"
                 v-model="userInfo.email"
                 name="メールアドレス"
                 type="text"
                 placeholder=""
                 @input="inputMail"
                 class="signin__input"/>
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
            <div class="signin__pass">
              <inputA
                 data-testid="inputPassword"
                 v-model="userInfo.password"
                 name="パスワード"
                 :type="inputType"
                 placeholder="*******"
                 @input="inputPassword"
                 class="signin__input"></inputA>
               <div data-testid="hidePassword" @click="hidePassword" class="signin__pass__icon ">
                 <div v-show="isChecked">
                   <img src="~/assets/img/eye_icon.webp">
                  </div>
                 <div v-show="!isChecked">
                   <img src="~/assets/img/noeye_icon.webp">
                  </div>
               </div>
            </div>
              <span>
                {{ errors[0] }}
              </span>
            </validation-provider>
          </div>
          <div class="signin__login">
            <round-bottun
            data-testid="roundButton"
            @click="login"
            :disabled="invalid"
            class="signin__login_btn">
              ログイン
            </round-bottun>
          </div>
        </ValidationObserver>
        <div class="signin__send">
          <router-link to="/signup">
            新規会員登録はこちら
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { ValidationProvider, ValidationObserver } from 'vee-validate';
import { userLoginType } from '../types/userInfoType';
import { UserStore } from '../store';
import { auth } from '../plugins/firebase';
import roundBottun from '../components/atoms/button/roundBottun.vue'
import inputA from '../components/atoms/input/inputA.vue'

type DataType = {
  userInfo: userLoginType;
  isChecked: boolean;
};
type headType = {
  title: string;
};

export default Vue.extend({
  middleware: 'authenticated',
  data(): DataType {
    return {
      userInfo: {
        email: '',
        password: '',
      },
      isChecked: false,
    };
  },
    head(): headType {
    return {
      title: 'ログイン',
    };
  },
  components: {
    ValidationProvider,
    ValidationObserver,
    roundBottun,
    inputA
  },
  methods: {
    login(): void {
        auth
          .signInWithEmailAndPassword(
            this.userInfo.email,
            this.userInfo.password
          )
          .then((authUser):void => {
            UserStore.loginAct(authUser.user!.email!, authUser.user!.uid).then(
              () => 
              console.log('login完了'),
              this.$router.push('/')
            );
          });
    },
    inputMail(newvalue:string): void {
      this.userInfo.email=newvalue
    },
    inputPassword(value:string): void {
      this.userInfo.password=value
    },
    hidePassword(): void {
      this.isChecked = !this.isChecked;
    }
  },
  computed: {
    inputType():string {
      return this.isChecked ? "text" : "password";
    },
  }});
</script>
<style lang="scss">
@import "../style/pages/signin.scss";
.signin {
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