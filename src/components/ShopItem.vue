<template>
  <div>
    <figure class="shopItem-img">
      <img :src="shop.image_url.shop_image1" alt="">
      <i class="fas fa-heart shopItem-icon"
        @click="onLiked"></i>
    </figure>
    <div class="shopItem-subinfo">
      <span class="shopItem-area">{{ shop.code.areaname_s }}</span>
    </div>
    <h2 class="shopItem-name">{{ shop.name }}</h2>
  </div>
</template>

<script>
export default {
  name: "ShopItem",
  components:{

  },
  props:{
    shop:Object
  },
  data(){
    return{

    };
  },
  methods:{
    onLiked(){
      this.$emit("on-liked", this.shop, !this.shop.liked)
    }
  }
}
</script>

<style scoped>


</style>