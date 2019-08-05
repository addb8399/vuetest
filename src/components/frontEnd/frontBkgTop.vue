<template>
    <div>
      <loading :active.sync="isLoading"></loading>
        <nav class="navbar navbar-light bg-light">
        <router-link class="navbar-brand" to="/products">
          <i class="fas fa-user-tie" aria-hidden="true"></i>
          金仕曼 Gentleman Shop
        </router-link>
    <!-- <a class="navbar-brand" href="shoppingCart.html">
      <i class="fas fa-user-tie" aria-hidden="true"></i>
      金仕曼 Gentleman Shop
    </a> -->
    <!-- 購物車內的數量 (Button 內包含 icon, 數量 badge) -->
    <div class="dropdown ml-auto">
      <router-link class="btn btn-sm btn-cart" to="/login">
        <i class="fas fa-user-cog text-dark fa-2x"></i>
      </router-link>
      <!-- <button >
        
      </button> -->
      <button class="btn btn-sm btn-cart" data-toggle="dropdown" data-flip="false"
        id="carmenu" @click="showcarORnot">
        <i class="fa fa-shopping-cart text-dark fa-2x" aria-hidden="true"></i>
        <span class="badge badge-pill badge-danger">{{cartData.length}}</span>
        <span class="sr-only">unread messages</span>
      </button>
      <div class="dropdown-menu dropdown-menu-right p-3" style="min-width: 400px" data-offset="400"
      :class="{show:showcar}"  aria-labelledby="carmenu">
        <h6>已選擇商品</h6>
        <table class="table table-sm">
          <tbody>
            <tr v-for="item in cartData" :key="item.id">
              <td class="align-middle text-center">
                <a href="#removeModal" class="text-muted" data-toggle="modal" data-title="刪除 金牌西裝 1 件" @click.prevent="delCartItem(item.id)">
                  <i class="far fa-trash-alt"></i>
                  <!-- <i class="fab fa-trash-o" aria-hidden="true"></i> -->
                </a>
              </td>
              <td class="align-middle">{{item.product.title}}</td>
              <td class="align-middle">{{item.qty}} {{item.product.unit}}</td>
              <td class="align-middle text-right">${{item.total}}</td>
            </tr>
            <!-- <tr>
              <td class="align-middle text-center">
                <a href="#removeModal" class="text-muted" data-toggle="modal" data-title="刪除 金牌女裝 1 件">
                  <i class="fa fa-trash-o" aria-hidden="true"></i>
                </a>
              </td>
              <td class="align-middle">金牌女裝</td>
              <td class="align-middle">1 件</td>
              <td class="align-middle text-right">$480</td>
            </tr> -->
          </tbody>
        </table>
        <a href="#" class="btn btn-primary btn-block" @click.prevent="gotoCheckOut()">
          <i class="fa fa-cart-plus" aria-hidden="true"></i> 結帳去
        </a>
      </div>
            <!-- <button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          Dropdown button
            </button>
            <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
              <a class="dropdown-item" href="#">Action</a>
              <a class="dropdown-item" href="#">Another action</a>
              <a class="dropdown-item" href="#">Something else here</a>
            </div> -->
    </div>
  </nav>
    </div>
</template>

<script>
export default {
  props:["cartData"],
  data(){
    return{
      isLoading:false,
      showcar:false,
    }
  },

  methods:{
    delCartItem(id){
      const api=`${process.env.APIPATH}/api/${process.env.COUSTOMPATH}/cart/${id}`;
      const vm=this;
      vm.isLoading=true;
      this.$http.delete(api).then((response) => {
      console.log(response)
      this.$emit('afterRemove',id)
      vm.isLoading=false;
      })
    },
    showcarORnot(){
      this.showcar= !this.showcar
    },
    gotoCheckOut(){
      const vm=this;
      console.log("gotoCheckOut")
      vm.$router.push(`/checkPage`)
    }
  },

}
</script>

