<template>
     <div
        class="page_top"
        v-show="scrollY > 300"
        @click="page_up"
     >
        <img src="../../../assets/img/page_top3.png" >
     </div>
</template>
<script lang="ts">
import Vue from 'vue';
type DataType = {
  scrollTimer:number,
  scrollY:number,
};
// import a from '../../../'
export default Vue.extend({
  data(): DataType {
    return {
      scrollTimer:0,
      scrollY:0,
    };
  },
  mounted(){
    window.addEventListener('scroll',this.handleScroll);
  },
  methods: {
    page_up() {
      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });
    },
    handleScroll:function(){
          if(this.scrollTimer){
            return;
          }
          //@ts-ignore
          this.scrollTimer = setTimeout(()=>{
            this.scrollY = window.scrollY;
            clearTimeout(this.scrollTimer);
            this.scrollTimer = 0;
          },100);
    },    
  }
})
</script>
<style lang="scss">
.page_top {
    position: fixed;
    right: 10px;
    bottom: 10px;
    width: 7rem;
    margin-bottom: 2rem;
    cursor: pointer;
    animation-name: slideIn ;
    animation-duration: 1s;
}
@keyframes slideIn {
  0% {
    opacity: 0;
    transform: translateY(64px);
  }
  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

</style>