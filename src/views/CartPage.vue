<template>
  <the-header class="shadow-sm"></the-header>
  <div class="container">
    <span v-on:keydown="key">{{ message }}</span>
    <span class="d-flex mb-3 mt-2">
      <router-link to="/">
        <span class="material-symbols-outlined"> arrow_back </span> Back To Shopping
      </router-link>
    </span>
    <div class="card" v-if="cart.length != 0">
      <table class="table cart">
        <thead>
          <tr>
            <th scope="col" class="w-10">Product ID</th>
            <th scope="col" class="w-50">Item</th>
            <th scope="col" class="w-10">Quantity</th>
            <th scope="col" class="w-10">Price/item</th>
            <th scope="col" class="w-20"></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="product in allItems" :key="product.id">
            <td>{{ product.id }}</td>
            <td class="d-flex">
              <img :src="product.img" class="product_img" />
              <span class="product_title">{{ product.title }}</span>
            </td>
            <td>
              <input type="number" name="" id="" @change="onChange(product.id, $event)" :value="product.count" size="4" min="1" max="10" />
            </td>
            <td><span>${{ convertToDollars(product.price) }}</span></td>
            <td>
              <button class="btn btn-danger" @click="deleteItem(product.id)">
                <span class="material-symbols-outlined"> delete_forever </span>
              </button>
            </td>
          </tr>

          <tr>
            <td></td>
            <td></td>
            <td>Total Quantity: {{ items }}</td>
            <td>Grand Total: ${{ convertToDollars(total) }}</td>
            <td><button class="btn btn-success" @click="checkout">Buy Now</button></td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="no-item" v-else>
      Nothing in the cart !
    </div>
  </div>
</template>


<script>
import { mapActions, mapMutations, mapState } from "vuex";
import TheHeader from "../components/TheHeader.vue";

export default {
  components: {
    TheHeader
  },
  created() {
    console.log("Created", this.cart);
    this.allItems = this.cart;
  },
  data() {
    return {
      allItems: [],
      exchangeRate: 0.75, // Assume exchange rate: 1 USD = 0.75 INR
      message: ""
    };
  },
  methods: {
    ...mapMutations("cart", ["SET_QUANTITY", "DEL_ITEM"]),
    onChange(id, e) {
      console.log(id, e.target.value);
      const count = e.target.value;
      if (count > 0 && count <= 10) {
        const arr = [id, count];
        this.SET_QUANTITY(arr);
        console.log("completed");
        this.$store.dispatch("cart/updateCart").then(result =>
          console.log("dispatched method")
        );
      } else {
        alert("invalid input");
      }
    },
    deleteItem(id) {
      console.log("in deleted!!");
      this.DEL_ITEM(id);
      this.$store.dispatch("cart/updateCart").then(result =>
        console.log("dispatched method")
      );
    },
    checkout() {
      console.log("isAuthenticated", this.$store.getters["user/isAuthenticated"]);
      if (!this.$store.getters["user/isAuthenticated"]) {
        console.log(this.$route);
        this.$router.push({ path: "/login", query: { redirectTo: this.$route.fullPath } });
      } else {
        console.log("done with checkout");
        let op = confirm("Do you want to continue ?");
        if (op) {
          this.$store.commit("cart/RESET_STATE");
          this.$store.dispatch("cart/updateCart").then(result =>
            console.log("dispatched method")
          );
        } else {
          alert("your purchase is not completed!!");
        }
      }
    },
    convertToDollars(price) {
      return (price * this.exchangeRate).toFixed(2);
    }
  },
  computed: {
    ...mapState("cart", ["cart"]),
    items() {
      let sum = 0;
      this.cart.forEach(each => {
        sum = sum + Number(each.count);
      });
      return sum;
    },
    total() {
      let sum = 0;
      this.cart.forEach(each => {
        sum = sum + each.price * each.count;
      });
      return sum;
    }
  }
};

</script>

<style scoped>
.container {
  height: 100vh;
}

table {
  text-align: center;
  border: 1px solid white; 
 
}


.card {
    background: radial-gradient(55.15% 55.15% at 50% 44.85%, rgba(255, 255, 255, 0.5) 0%, rgba(255, 255, 255, 0) 100%), radial-gradient(42.29% 100% at 85.91% 0%, #30E8FF 0%, #95EBF5 23.44%, rgba(255, 255, 255, 0) 100%), radial-gradient(100% 100% at 50% 0%, #FFA2B3 0%, rgba(255, 206, 215, 0.520833) 48.44%, rgba(255, 255, 255, 0) 100%), radial-gradient(93.09% 93.09% at 12.86% 8.24%, rgba(255, 164, 29, 0.4) 0%, rgba(255, 255, 255, 0) 100%);
    max-height: 500px;
    background-repeat: no-repeat;
    border: 1px solid white;
   
}

.cart img {
  width: 70px;
  height: 70px;
}

.card-image {
  max-height: 200px;
  padding: 25px;
}
.card-text {
  font-size: 13px;
}
.card-title {
  font-size: 18px;
  font-weight: 600;
  max-block-size: 20px;
  overflow: hidden;
}
.card-title:hover {
  text-decoration: underline;
  cursor: pointer;
}
button {
  display: flex !important;
  justify-content: center;
}
.card:hover {
  transform: translate(0, -1px);
}
.card-body {
  margin-top: 0px;
}
.desc {
  max-block-size: 40px;
  overflow: hidden;
}
.item2 {
  flex: 0 0 33%;
}

input {
  border: 1px solid rgb(248, 248, 248);
  background-color: rgb(255, 255, 255);
  border-radius: 4px;
  padding: 4px;
}
tr {
  vertical-align: middle;
}
.no-item{
  margin:auto;
  font-size: 35px;
  text-decoration: none;
}

.product_title{
  margin-left: 40px;
  margin-top: 15px;
  font-weight: bold;
}

.product_img{
  border-radius: 50%;
}
</style>
