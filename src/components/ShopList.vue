<template>
  <section class="shopList-wrapper" v-cloak>
    <transition-group class="shopItems" name="shop-item" v-if="filteredShops.length" tag="div">
      <div 
        class="shopItem"
        v-for="shop in filteredShops"
        :class="{active: shop.liked}"
        :key="shop.id">

        <shop-item 
          :shop="shop"
          @on-liked="onLiked"></shop-item>
        
      </div>
    </transition-group>
    <div v-else>
      <p class="noshop-wrapper" v-if="shops.length">
        <img class="noshop" src="./../assets/noshop.png" alt="">
        <span class="noshop-txt">お気に入りに登録されているお店がありません。</span>
      </p>
      <p class="noshop-wrapper" v-else>
        <img class="noshop" src="./../assets/search.png" alt="">
        <span class="noshop-txt">お気に入りのお店見つかるかな〜〜</span>
      </p>
    </div>
  </section>
</template>

<script>
import ShopItem from "./ShopItem.vue";

export default {
  name: "ShopList",
  components:{
    ShopItem
  },
  props:{
    shops: Array,
    filteredShops: Array
  },
  data(){
    return{
    };
  },
  methods:{
    onLiked(shop, liked){
      this.$emit("on-liked", shop, liked)
    }
  }
}
</script>

<style>
.shopList-wrapper{
  margin-top: 30px;
  margin-bottom: 30px;
  width: 100%;
}
.shopItems{
  display: flex;
  flex-wrap: wrap;
  position: relative;
  margin-left: -20px;
}
.shopItem{
  width: calc( 100% / 4 - 20px);
  margin-left: 20px;
  margin-bottom: 20px;
  transition: 1s;
}
@media screen and (max-width: 768px) {
  .shopItem{
    width: calc( 100% / 3 - 20px);
  }
}
@media screen and (max-width: 480px) {
  .shopItem{
    width: calc( 100% / 2 - 20px);
  }
}
.shop-item-enter, .shop-item-leave-to{
  opacity: 0;
  transform: translateX(200px);
}
.shop-item-leave-active{
  position: absolute;
}
.shopItem-img{
  position: relative;
}
.shopItem-img:before{
  content: "";
  display: block;
  padding-top: 100%;
}
.shopItem-img:after{
  content: "";
  display: block;
  position: absolute;
  z-index: 1;
  right: -2px;
  bottom: -2px;
  width: 50px;
  height: 50px;
  background: url(./../assets/heart_back.png) no-repeat;
}
.shopItem-img img{
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  object-fit: cover;
  -webkit-box-shadow: 0 3px 6px rgba(0,0,0,.16);
  box-shadow: 0 3px 6px rgba(0,0,0,.16);
  border-radius: 5px;
}
.shopItem-subinfo{
  display: flex;
  justify-content: space-between;
}
.shopItem-area{
  font-size: 10px;
  padding: 3px 5px;
  background-color: #326432;
  color: #fff;
}
.shopItem-icon{
  font-size: 20px;
  cursor: pointer;
  position: absolute;
  bottom: 5px;
  right: 5px;
}
.shopItem-subinfo{
  margin-top: 5px;
}
.shopItem-name{
  margin-top: 5px;
  font-size: 14px;
}
.noshop{
  max-width: 200px;
  margin-top: 50px;
}
.noshop-txt{
  display: block;
  margin-top: 20px;
  font-size: 14px;
}
.noshop-wrapper{
  text-align: center;
}

.shopItem .shopItem-icon{
  position: absolute;
  bottom: 5px;
  right: 5px;
  z-index: 2;
  color: #ddd;
  filter: drop-shadow(0px 0px 1px rgba(0,0,0,0.1));
  font-size: 20px;
  cursor: pointer;
}
.active .shopItem-icon{
  color:#ed828f;
}
</style>