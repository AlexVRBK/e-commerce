<template>
<the-header class="shadow-sm"></the-header>
<div class="layout">
   <div class="section1">
      <p class="sidebar-title">CATEGORIES</p>
      <ul v-for="item in categoryList" :key="item">
         <li @click="filteredItems(item)">{{item}}</li>
      </ul>
   </div>
   <div class="section2">
      <div class="item2" v-for="product in ProductList" :key="product.id">
         <item-card :product="product"></item-card> 
      </div>  
   </div>
</div>
</template>

<script>
import ItemCard from "../components/ItemCard.vue"
import TheHeader from "../components/TheHeader.vue"
import { mapState } from "vuex"
export default {
components:{
   ItemCard,
   TheHeader
   
},
data() {
   return {
      categoryList:[],
      ProductList:[],
      allProducts:[]
   }
},
created() {
   this.loadCategories()
   this.loadAllProducts()
},
computed:{
   ...mapState('cart', ['cart']),
   cart(){
    console.log("cart chanf=ged")
   }
},
methods:{
   loadCategories(){
      fetch('https://fakestoreapi.com/products/categories')
            .then(res=>res.json())
            .then(json=>this.categoryList = json)
   },
   loadAllProducts(){
      fetch('https://fakestoreapi.com/products')
            .then(res=>res.json())
            .then((json)=>{
               this.ProductList = json
               this.allProducts = json
               })
   },
   filteredItems(category){
      const filteredItems = this.allProducts.filter((item)=>{
        return item.category == category;
      })
      this.ProductList = filteredItems
   }
}

}
</script>

<style>
.layout{
   display: grid;
   gap:1.5rem;
   grid-template-areas: "section1 section2";
   grid-template-columns: 1fr 5fr;
     background: radial-gradient(55.15% 55.15% at 50% 44.85%, rgba(255, 255, 255, 0.5) 0%, rgba(255, 255, 255, 0) 100%), radial-gradient(42.29% 100% at 85.91% 0%, #30E8FF 0%, #95EBF5 23.44%, rgba(255, 255, 255, 0) 100%), radial-gradient(100% 100% at 50% 0%, #FFA2B3 0%, rgba(255, 206, 215, 0.520833) 48.44%, rgba(255, 255, 255, 0) 100%), radial-gradient(93.09% 93.09% at 12.86% 8.24%, rgba(255, 164, 29, 0.4) 0%, rgba(255, 255, 255, 0) 100%);
    max-height: 500px;
    background-repeat: no-repeat;
}

.section2{
   display: flex;
   flex-wrap: wrap;
   padding-top: 20px;
   max-width: 1200px;
   
}
.card{
   flex:0 0 30%;
   margin:10px;
}

.sidebar-title{
   text-align: center;
   font-size: 20px;
   margin-top: 25px;
   font-weight: bold;
}




.section1 > 

ul{
  text-transform: uppercase;
   list-style-type: none;
   text-align: center;
}

.section1 > ul:hover {
   border: 1px solid #000;
   background-color: #000;
   color: #fff;
   cursor: pointer;
   transition: all .1s ease-in;
}
.section1 > ul{
   border: 1px solid rgb(236, 232, 232);
   margin-left: 25px;
   padding: 5px;
  border-radius: 2.1875em 0 0 2.1875em;
}
</style>