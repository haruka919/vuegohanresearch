<template>
  <div class="search-wrapper" :class="{display: visibility === 'liked'}">
    <input 
      type="search" 
      class="search-input" 
      placeholder="場所・ジャンルを検索"
      v-model="keyword">
    <button 
      type="submit" 
      class="search-btn"
      @click="shopSearch"><i class="fas fa-search"></i></button>   
  </div>
</template>

<script>
export default {
  name: "ShopSearch",
  props:{
    visibility: String
  },
  data(){
    return{
      icon: '\uf002',
      keyword: ''
    };
  },
  methods:{
    shopSearch(){
      this.$emit('shop-search', this.keyword);
      this.keyword = '';
    }
  }
}
</script>

<style scoped>
.search-wrapper.display{
  display: none;
}
</style>