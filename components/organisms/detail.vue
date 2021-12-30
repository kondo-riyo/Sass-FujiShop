<template>
  <div class="detail">
    <div class="detail__img">
      <div>
        <img :src="itemDetail.img" />
      </div>

      <rings @selectRing="changeImg" :item="itemDetail" />
    </div>
    <div
      class="detail__msg"
    >
      <div>
        <p class="detail__msg__name">
          {{ itemDetail.name }}
        </p>
      </div>
      <div class="detail__msg__price ">
        <p>
          <span>￥{{ itemDetail.price }}</span>
          税込
        </p>
      </div>
      <div>
        <p class="detail__msg__ex">
          {{ itemDetail.description }}
        </p>
      </div>

      <div class="detail__num">
        <div>
          <span class="detail__num__font">
            <font style="vertical-align: inherit">
              <font style="vertical-align: inherit">個数</font>
            </font>
          </span>
          <div class="detail__num__select ">
            <select
              @change="updateValue"
            >
              <option v-for="(num, index) in options" :key="index" :value="num">
                {{ num }}
              </option>
            </select>
            <span>
              <svg
                fill="none"
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                viewBox="0 0 24 24"
              >
                <path d="M6 9l6 6 6-6"></path>
              </svg>
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import rings from '../morecules/rings.vue';
type DataType = {
  selectedItemNum: number;
};
export default Vue.extend({
  components: { rings },
  data(): DataType {
    return {
      selectedItemNum: 1,
    };
  },
  props: ['itemDetail', 'options'],
  methods: {
    updateValue(e: string): void {
      //@ts-ignore
      this.$emit('input', e.target.value);
    },
    changeImg(img: string): void {
      this.itemDetail.img = img;
    },
  },
  created() {
    this.$emit('input', 1);
  },
});
</script>
<style lang="scss">
  @import "../../style/organisms/detail.scss";

</style>
