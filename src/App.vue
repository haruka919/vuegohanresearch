<template>
  <div id="app">
    <header class="header">
      <div class="header-wrapper">
        <div class="header-inner">
          <img class="header-title__iconL" src="./assets/title_L.svg" alt="スプーン">
          <h1 class="header-title">Vue.jsでグルメ検索</h1>
          <img class="header-title__iconR" src="./assets/title_R.svg" alt="フォーク">    
        </div>
      </div>
    </header>
    <main>
      <div class="site-width">
        <shop-search
          @shop-search="shopSearch"
          :visibility="visibility"></shop-search>

        <shop-list
          :shops="shops" 
          :filtered-shops="filteredShops"
          @on-liked="onLiked"></shop-list>
      </div>

      <footer>
        <div class="footer-wrapper" v-if="shops.length">
          <a href="#/all" class="footer-icon-wrapper" v-if="visibility === 'liked'">
            <i class="fas fa-home footer-icon"></i>
            <span class="footer-icon-txt">戻る</span>
          </a>
          <a href="#/liked" class="footer-icon-wrapper" v-else>
            <i class="far fa-heart footer-icon"></i>
            <span class="footer-icon-txt">お気に入り</span>
          </a>
        </div>
        <div class="footer-wrapper" v-else>
          <a class="footer-icon-wrapper non-click">
            <i class="far fa-heart footer-icon"></i>
            <span class="footer-icon-txt">お気に入り</span>
          </a>
        </div>
      </footer>
    </main>
  </div>
</template>

<script>
import ShopSearch from "./components/ShopSearch.vue";
import ShopList from "./components/ShopList.vue";
import axios from "axios"

var STORAGE_KEY = 'shops'
var shopStorage = {
  fetch: function() {
    var shops = JSON.parse(
      localStorage.getItem(STORAGE_KEY) || '[]'
    )
    shops.forEach(function(shop, index) {
      shop.id = index
    })
    shopStorage.uid = shops.length
    return shops
  },
  save: function(shops) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(shops))
  }
}

const filters = {
  all(shops){
    return shops;
  },
  active(shops) {
		return shops.filter((shop) => !shop.liked);
	},
  liked(shops){
    return shops.filter((shop) => shop.liked);
  }
}

export default {
  name: "app",
  components:{
    ShopSearch,
    ShopList,
  },
  data(){
    return{
      shops: shopStorage.fetch(),
      visibility: 'all',
      apidata:[]
    }
  },
  computed:{
    filteredShops(){
      return filters[this.visibility](this.shops);
    }
  },
  mounted() {
		window.addEventListener('hashchange', this.onHashChange);
	},
  watch: {
    shops: {
			handler(shops) {      
        shopStorage.save(shops);        
			},
			deep: true
    }
  },
  methods: {
    onLiked(shop, liked){
      shop.liked = liked;
    },
    shopSearch(value){
      const params = { 
        keyid: 'eeba67169bdf61e138c03f580dffeace',
        hit_per_page: 12,
        freeword: value,
      };
      axios.get('https://api.gnavi.co.jp/RestSearchAPI/v3/',{params})
      .then(response => {

        this.apidata = response.data.rest;
        for(let i = 0; i < this.apidata.length; i++) {
          this.$set(this.apidata[i], "liked", false);
        }  
        // Array.prototype.push.apply(this.shops, this.apidata);
        this.shops = this.shops.concat(this.apidata);
        this.shops.reverse();
        // console.log(this.shops);
        // console.log(b);
        
        // this.shops = response.data.rest;
        
        // for(let i = 0; i < this.shops.length; i++) {
        //   this.$set(this.shops[i], "liked", false);
        // }
      
      });    
    },
    onHashChange(){
      const visibility = window.location.hash.replace(/#\/?/, '');
      this.visibility = visibility;
    }
  }
}
</script>

<style>
@charset "utf-8";
/*
 Share Style [
----------------------------------------------------------- */
html {
	overflow-y:scroll;
}
body{
  font-family:'メイリオ', Meiryo,'TsukuARdGothic-Regular','Hiragino Kaku Gothic ProN','ヒラギノ角ゴ ProN W3',sans-serif;
  font-size: 16px;
  color: #333;
}
*, *::before, *::after {
	margin:0;
  padding:0;
  box-sizing: border-box;
}
h1,h2,h3,h4,h5,h6 {
}
p,li,dt,dl {
	line-height:1.6;
}
p {
	margin:0 0 1em 0;
}
li {
	list-style-type:none;
}
/*
 Link Color [
----------------------------------------------------------- */
a:link {
	color:#078D00;
	text-decoration:none;
}
a:visited {
	color:#189B12;
	text-decoration:none;
}
a:hover {
	text-decoration:underline;
}
a:active {
	color:#666;
}
.site-width{
  max-width: 800px;
  width: 90%;
  margin: 0 auto;
  min-height: calc( 100vh - 170px);
}
img{
  width: 100%;
  vertical-align: middle;
}
input[type="text"] {
  padding: 0;
  border: none;
  border-radius: 0;
  outline: none;
  background: none;
}
button,
input[type="submit"],
input[type="search"] {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  padding: 0;
  border: none;
  outline: none;
  background: transparent;
}
#app{
  background-color: #ECEAE4;
}
/*
 header [
----------------------------------------------------------- */
.header{
  width: 100%;
  height: 70px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0,0,0,.5);
  display: flex;
  justify-content: center;
  align-content: center;
}
.header-wrapper{
  width: 90%;
  margin: 0 auto;
  text-align: center;
  height: 70px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.header-inner{
  width: 400px;
  position: relative;
}
.header-title__iconL{
  position: absolute;
  left: 0;
  width: 18px;
  transform: rotate(-12deg);
}
.header-title__iconR{
  width: 18px;
  position: absolute;
  right: 0;
  transform: rotate(12deg);
}
.header-title{
  font-size: 24px;
  font-weight: bold;
  color:#333;
  display: inline-block;
  font-family: ten-mincho, serif;
  font-weight: 400;
  font-style: normal;
}
/*
 main [
----------------------------------------------------------- */
.search-wrapper{
  position: relative;
  margin-top: 30px;
}
input[type="search"].search-input{
  font-size: 16px;
  width: 100%;
  flex: 1;
  display: inline-block;
  font-family: FontAwesome;
  font-style: normal;
  font-weight: normal;
  line-height: 1;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: 5px 15px;
  background-color: #f1f1f1;
  height: 60px;
  border-radius: 10px;
  -webkit-box-shadow: 0 3px 6px rgba(0,0,0,.16);
  box-shadow: 0 3px 6px rgba(0,0,0,.16);
}
button.search-btn{
  background-color: #BA1B2D;
  color: #fff;
  font-size: 24px;
  font-weight: bold;
  padding: 15px 17px;
  cursor: pointer;
  border-radius: 0 10px 10px 0;
  height: 60px;
  width: 58px;
  -webkit-box-shadow: 0 3px 6px rgba(0,0,0,.16);
  box-shadow: 0 3px 6px rgba(0,0,0,.16);
  position: absolute;
  right: 0;
}
button.search-btn:hover{
  opacity: 0.8;
}

/*
 footer [
----------------------------------------------------------- */
.footer-wrapper{
  width: 100%;
  height: 70px;
  box-shadow: 0 0 20px rgba(0,0,0,.1);
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #fff;
}
.footer-icon-wrapper{
  display: flex;
  align-items: center;
  flex-direction: column;
  cursor: pointer;
}
.footer-icon-wrapper:hover{
  text-decoration: none;
}
.footer-icon{
  font-size: 25px;
  color: #333;
}
.footer-icon-txt{
  font-size: 10px;
  padding-top: 5px;
  color: #333;
}
.non-click{
  pointer-events: none;
}
</style>
