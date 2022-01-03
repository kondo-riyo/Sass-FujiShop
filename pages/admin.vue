<template>
<div class="admin" v-if='userFromStore'>
    <!-- 管理者画面ログイン password=123456-->
    <div 
     v-show="userFromStore.uid!='NAkxF849wXcbaIf9gdvBIINweOi1' && !adminPassword"
     class="admin__login"
    >
    <div class="admin__login__card">
        <div class="admin__login__card__title ">管理者パスワード</div>
        <div class="admin__login__card__input">
            <input-a
            name="パスワード"
            type="password"
            placeholder=""
            v-model="adminPassword_num"
            class="admin__login__card__input-a"
            />
            <button @click="adminPassword_push">
            <img 
            src="~/assets/img/yajirusi_icon.webp" 
            class="admin__login__card__input-yaji "
            >
            </button>
        </div>
    </div>
    </div>
    <!-- 通常画面 -->
    <div 
     v-show="userFromStore.uid==='NAkxF849wXcbaIf9gdvBIINweOi1' || adminPassword"
     class=""
     id="admin_show"
    >
        <div class="admin__page">
        <div class="admin__page__name">
            ようこそ！ {{userFromStore.name}} さん
        </div>
        <div class="admin__page__msg">
            ここでは各ユーザーの注文履歴の確認、配達状況の変更ができます
        </div>
        </div>
        <div class="admin__userInfo">
            ユーザー情報
        </div>
        <div class="admin__table ">
            <div class="admin__table__title">
                <div class="admin__table__title_no">No.</div>
                <div class="admin__table__title_name">名前</div>
                <div class="admin__table__title_id">ID</div>
                <div class="wadmin__table__title_em"></div>
            </div>
        <div class="admin__table__body">
        <div class="admin__table__body_base">
            <div class="admin__table__body_back">
            <div 
            v-for="(user, index) in adminFromStore" :key="user.orderId"
            class="admin__table__stick"
            @click="userLog(user.uid)"
            >
                    <div class="admin__table__title_no">{{index+1}}</div>
                    <div class="admin__table__title_name">{{user.name}}</div>
                    <div class="admin__table__title_id">{{user.uid}}</div>
                    <div class="admin__table__title_em">
                        <img src="~/assets/img/pencil_icon.webp">
                    </div>
            </div>
            </div>
        </div>
        </div>
    </div>
    </div>
</div>
</template>
<script lang="ts">
import Vue from 'vue'
import { UserStore, AdminStore } from '../store'
import inputA from '../components/atoms/input/inputA.vue';
import RoundBottun from '../components/atoms/button/roundBottun.vue';
import { idNameType, userInfoType } from '../types/userInfoType';

type DataType = {
    adminPassword: boolean;
    adminPassword_num: string;
};
type headType = {
  title: string;
};

export default Vue.extend({
  components: { inputA, RoundBottun },
    head(): headType {
        return {
        title: '注文管理(管理者)',
        };
    },
    data():DataType {
        return{
            adminPassword: false,
            adminPassword_num: ''
        }
    },
    async fetch(): Promise<void> {
        await AdminStore.fetchUsersAct();
    },
    computed:{
        userFromStore(): userInfoType|null {
            return UserStore.getUserInfo
        },
        adminFromStore(): idNameType[] {
            return AdminStore.getUsersList
        }
    },
    methods:{
        userLog(uid: string|undefined):void {
            this.$router.push({name: 'adminDetail-adminId', params: { adminId: uid } })
        },
        adminPassword_push():void {
            if(this.adminPassword_num === '123456') {
                this.adminPassword = true
            } else {
                this.adminPassword = false
            }
        },
    }
})
</script>
<style lang="scss">
@import "../style/pages/admin.scss";
@import "../style/main.scss";
.admin {
  background-color: $base_gray--25;
  height: 200px;
  @media (min-width: 640px) {
    background-image: url("../assets/img/coffee-2734137_1280.webp");
  }
}
</style>