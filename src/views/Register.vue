<template>
   <div class="container">
    <div class="logo">
      <router-link to="/home">
        <img src="@/assets/logo.svg" alt="">
      </router-link>
    </div>
    <div class="layout-login">
          <div class="card">
              <div class="card-header">Register</div>
              <form @submit.prevent="getRegister">
                <label for="name">Name</label>
                <div class="card-text"><input type="text" id="name" v-model="form.name" required></div>
                  <label for="email">Email</label>
                  <div class="card-text"><input type="email" id="email" v-model="form.email" required></div>
                  
                  <label for="pass">Password</label>
                  <div class="card-text"><input type="password" id="pass" v-model="form.pass"></div>
                  <button type="submit" class="btn btn-primary">Register</button>
              </form>
              <div class="link"><router-link to="/login">Already have an account? Login</router-link></div>
          </div>
      </div>
  </div>
</template>

<script>

export default {
    data() {
        return {
            form:{
                name:'',
                pass:'',
                email:''
            }
        }
    },
  methods:{
      getRegister(){
          this.$store.dispatch('user/signup', this.form)
            .then((res) => {
            let redirectTo = this.$route.query.redirectTo;
            if (redirectTo) {
            this.$router.replace({ path: redirectTo });
            }
            this.$router.push('/home');
           return res;
        })
        .catch((err) => {
          console.log("error in login");
        }); 
      }
  }  
}
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  height: 100vh;
  padding-top: 20px;
  max-width: 1000px;
  margin: 0 auto;
}

.logo {
  margin-bottom: 60px;
  margin-top: 20px;
  margin-right: 20px;
}
.logo img {
    margin: 0;
    min-width: 100px;
}
.card {
  margin: auto;
  min-width: 500px;
  padding: 10px;
  background: radial-gradient(55.15% 55.15% at 50% 44.85%, rgba(255, 255, 255, 0.5) 0%, rgba(255, 255, 255, 0) 100%), radial-gradient(42.29% 100% at 85.91% 0%, #30E8FF 0%, #95EBF5 23.44%, rgba(255, 255, 255, 0) 100%), radial-gradient(100% 100% at 50% 0%, #FFA2B3 0%, rgba(255, 206, 215, 0.520833) 48.44%, rgba(255, 255, 255, 0) 100%), radial-gradient(93.09% 93.09% at 12.86% 8.24%, rgba(255, 164, 29, 0.4) 0%, rgba(255, 255, 255, 0) 100%);
  max-height: 500px;
  background-repeat: no-repeat;
  border: 1px solid white;
  border-radius: 10px;
}

.card-header {
  text-align: center;
  font-size: 18px;
  font-weight: 600;
}

.card-text {
  margin-bottom: 10px;
}

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 10px;
}

.btn {
  display: block;
  width: 100%;
  padding: 8px;
  margin-top: 10px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.link {
  text-align: center;
  margin-top: 10px;
}

.link a {
  text-decoration: none;
}
</style>

