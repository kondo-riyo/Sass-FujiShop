<template>
  <div class="swipe">
    <div class="swipe__base">
      <div class="swipe__img">
        <router-link
          :to="{
            name: `${routerName}`,
            params: { itemId: swipeItem.id },
          }"
        >
          <img
            class=""
            :src="swipeItem.img"
            alt="Colors"
          />
        </router-link>
        <p v-if="swipeItem.rank">
          {{ swipeItem.rank }}位
        </p>
      </div>
      <div class="swipe__msg">
        <h1>
          {{ swipeItem.name }}
        </h1>
        <rings @selectRing="changeImg" :item="swipeItem" />
        <div class="swipe__msg__price">
            <div>
              <img src="~/assets/img/yenIcon.webp" />
            </div>
            <div>{{ swipeItem.price }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import rings from '../morecules/rings.vue';

export default Vue.extend({
  components: { rings },
  props: ['swipeItem', 'routerName'],
  methods: {
    changeImg(img: string): void {
      this.swipeItem.img = img;
    },
  },
});
</script>
<style lang="scss">
  @import "../../style/organisms/swipeCard.scss";

</style>