<template>
  <div class="search_group">
    <input-search @input="inputKeyWord"/>
    <search-logo @search="search" />
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import inputSearch from "../atoms/input/inputSearch.vue"
import searchLogo from "../atoms/logo/searchLogo.vue"

type DataType = {
  keyword: string
};

export default Vue.extend({
  components:{
    inputSearch,
    searchLogo
  },
  data(): DataType{
    return{
      keyword:''
    }
  },
  methods: {
    search(): void {
      this.$emit('searchItems',this.keyword);
    },

    inputKeyWord(keyWord:string):void{
      this.keyword = keyWord;
    },
  },
});
</script>
<style lang="scss">
.search_group {
  display: flex;
  align-items: center;
  margin: 0px 1.5rem;
}
</style>
