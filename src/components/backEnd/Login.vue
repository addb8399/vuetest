<template>
    <div>
        <div>
            <form class="form-signin" @submit.prevent="login">
                <img class="mb-4" src="/docs/4.3/assets/brand/bootstrap-solid.svg" alt="" width="72" height="72">
                <h1 class="h3 mb-3 font-weight-bolder text-center">管理員登入</h1>
                <label for="inputEmail" class="sr-only">Email address</label>
                <input type="email" id="inputEmail" v-model="user.username" class="form-control" placeholder="Email address" required autofocus>
                <label for="inputPassword" class="sr-only">Password</label>
                <input type="password" id="inputPassword" v-model="user.password" class="form-control" placeholder="Password" required>
                <div class="checkbox mb-3">
                    <label>
                    <input type="checkbox" value="remember-me"> Remember me
                    </label>
                </div>
                <button class="btn btn-lg btn-primary btn-block" type="submit">Sign in</button>
                <button class="btn btn-lg btn-primary btn-block" @click="gotoProducts">返回賣場</button>
                <!-- <router-link class="btn btn-lg btn-primary btn-block" to="/products">返回賣場</outer-link> -->
                <p class="mt-2 mb-3 text-muted">&copy; 2017-2019</p>
                
            </form>
                
        </div>
    </div>
</template>
<script>
export default {
  data () {
    return {
      user:{
        username:'',
        password:'',
      }
    }
  },
  methods:{
    login(){
      const api=`${process.env.APIPATH}/admin/signin`
      const vm=this
      this.$http.post(api,vm.user).then((response) => {
      console.log(response)
      if(response.data.success){
        vm.$router.push('/admin/productList')
      }
    })
    },
    gotoProducts(){
      const vm=this
      vm.$router.push('/products')
    }
  },

}
</script>

<style scoped>
html,
body {
  height: 100%;
}

body {
  display: -ms-flexbox;
  display: flex;
  -ms-flex-align: center;
  align-items: center;
  padding-top: 40px;
  padding-bottom: 40px;
  background-color: #f5f5f5;
}

.form-signin {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
}
.form-signin .checkbox {
  font-weight: 400;
}
.form-signin .form-control {
  position: relative;
  box-sizing: border-box;
  height: auto;
  padding: 10px;
  font-size: 16px;
}
.form-signin .form-control:focus {
  z-index: 2;
}
.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
</style>
