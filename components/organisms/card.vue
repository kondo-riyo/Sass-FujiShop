<template>
  <div class="card">
    <div class="card__div">
      <router-link
        :to="{
          name: `${routerName}`,
          params: { itemId: item.id },
        }"
      >
        <img class="card__div__img" :src="item.img" />
      </router-link>
    </div>

    <div class="card__msg">
      <div >
        <rings @selectRing="changeImg" :item="item" />
        <h1 class="card__msg__name">
          {{ item.name }}
        </h1>
      </div>
      <div class="card__msg__price">
        <span class="card__msg__price__span">{{ item.price }}円(税込)</span>
        <span>
          <router-link
            :to="{
              name: `${routerName}`,
              params: { itemId: item.id },
            }"
          >
            <round-bottun class="card__msg__price__detail"> 詳細 </round-bottun>
          </router-link>
        </span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import rings from '../morecules/rings.vue';
import roundBottun from '../atoms/button/roundBottun.vue';

export default Vue.extend({
  components: { rings, roundBottun },
  props: ['item', 'routerName'],
  methods: {
    changeImg(img: string): void {
      this.item.img = img;
    },
  },
});
</script>
<style lang="scss">
  @import "../../style/organisms/card.scss";

</style>