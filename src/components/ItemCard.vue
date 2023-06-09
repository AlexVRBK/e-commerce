<template>
  <div class="card shadow-sm">
    <img class="card-img-top card-image" :src="product.image" alt="Card image cap">
    <div class="card-body">
      <h5 class="card-title" @click="productDetails(product.id)">{{product.title}}</h5>
      <h5 class="card-title card-price">USD ${{ (product.price * exchangeRate).toFixed(2) }}</h5>
      <p class="card-text desc">{{ product.description }}</p>
      <p class="card-text ">{{product.category}}</p>
      <p class="card-text"><small class="text-muted">Rating: {{product.rating.rate}}/5</small></p>
      <div class="d-flex" style="justify-content: space-around;">
        <button class="btn btn-warning btn-text" type="button" @click="addToCart()">Add to Cart <span class="material-symbols-outlined">add_shopping_cart</span></button>
        <button class="btn btn-primary" type="button">Buy Now!<span class="material-symbols-outlined">shopping_bag</span></button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['product'],
  created() {
    // Отримання курсу обміну з сервера або іншими засобами
    this.exchangeRate = 0.75; // Припустимо, що курс 1 USD = 0.75 INR
  },
  data() {
    return {
      exchangeRate: 0,
    };
  },
  methods: {
    productDetails(id) {
      this.$router.push(`/details/${id}`);
    },
    addToCart() {
      alert(`${this.product.title} is Added to cart!!`);
      const product = {
        id: this.product.id,
        title: this.product.title,
        img: this.product.image,
        price: this.product.price,
      };
      this.$store.commit('cart/SET_ITEM', product);
      this.$store.dispatch('cart/updateCart').then((result) => console.log("dispatched method"));
    },
  },
};
</script>


<style>
.card{
  height: 500px;
  border-radius: 10%;
}
.card-image{
  height: 250px;
  max-width: 300px;
  padding: 25px;
  text-align: center;
  margin-left: 35px;
}
.card-text{
  font-size: 13px;
}

.card-title{
  font-size: 18px;
  font-weight: 600;
  max-block-size: 20px;
  overflow: hidden;
  text-overflow: ellipsis;
}
.card-title:hover{
  text-decoration: underline;
  cursor: pointer;
}
button{
display: flex !important;
justify-content: center;
}
.card:hover{
   transform: translate(0, -5px);
   border:1px solid rgb(255, 255, 255)
}
.card-body{
  margin-top: 0px;
}
.desc{
    max-block-size: 40px;
  overflow: hidden;
  text-overflow: ellipsis;
}
.item2{
  flex:0 0 33%;
}

.material-symbols-outlined{
  margin-left: 7px;
}


</style>