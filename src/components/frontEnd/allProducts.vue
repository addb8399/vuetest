<template>
    <div >
        <loading :active.sync="isLoading"></loading>
        <frontBkgTop :cartData=cartList @afterRemove="gerCart"></frontBkgTop>
        <div class="jumbotron jumbotron-fluid jumbotron-bg d-flex align-items-end">
            <div class="container">
            <div class="p-3 bg-lighter">
                <h1 class="display-4 font-weight-bold">「禮儀，成就不凡的男人」</h1>
                <p class="lead"> "Manners Maketh Man" by Harry Hart</p>
            </div>
            </div>
        </div>
        
  <!-- 列表頁 -->
  <div class="container main-contant mb-1">
    <div class="row">
      <div class="col-md-3">
            <!-- 左側選單 (List group) -->
            <div class="list-group sticky-top">

                <a href="#" class="list-group-item list-group-item-action" 
                    :class="{'active':choosetype=='all'}" @click.prevent="choosetype='all'" @click="removeSearchName">
                    <i class="fas fa-bookmark" aria-hidden="true"></i> 全部商品</a>

                <a href="#" class="list-group-item list-group-item-action" 
                    :class="{'active':choosetype=='suit'}" @click.prevent="choosetype='suit';removeSearchName()">
                    <i class="far fa-bookmark" aria-hidden="true"></i> 金仕曼西裝</a>                    

                <a href="#" class="list-group-item list-group-item-action" 
                    :class="{'active':choosetype=='hat'}" @click.prevent="choosetype='hat';removeSearchName()">
                    <i class="fas fa-bookmark" aria-hidden="true"></i> 金仕曼帽子</a>
                    
                <a href="#" class="list-group-item list-group-item-action"
                    :class="{'active':choosetype=='shoes'}" @click.prevent="removeSearchName();choosetype='shoes'">
                    <i class="far fa-bookmark" aria-hidden="true"></i> 金仕曼鞋子</a>

                <a href="#" class="list-group-item list-group-item-action"
                    :class="{'active':choosetype=='others'}" @click.prevent="choosetype='others';removeSearchName()">
                    <i class="fas fa-bookmark" aria-hidden="true"></i> 金仕曼配件</a>

            </div>
      </div>
      <div class="col-md-9">
        <div class="d-flex mb-4 justify-content-between">
          <!-- Search bar -->
          <form class="form-inline my-3 my-lg-0">
            <div class="input-group">
              <input class="form-control" type="text" placeholder="Search" aria-label="Search" v-model="searchnameReady" >
              <div class="input-group-btn">
                <button class="btn btn-outline-warning" type="submit" @click="searchProducts()">
                  <i class="fa fa-search" aria-hidden="true"></i> Search</button>
              </div>
            </div>
          </form>
          <div class="d-flex">
            <label for="onePageData"  class=" col-form-label" style="padding-right:10px" >一頁商品數量</label>
            <select name="" class="form-control mr-1" id="onePageData" v-model="page.onePageData" @change="page.current_page=1" style="width:38%" >
            <option value="6" page.onePageData>6個</option>
            <option value="9" >9個</option>
          </select>
          </div>
        </div>
        <!-- 主要商品列表 (Card) -->
        <div class="tab-content">
          <div class="tab-pane active" id="list-gold">
            <div class="row" v-if="productsType.length!=0">
              <!-- 金牌 -->
              <div class="col-md-4 mb-4" v-for="item in productsType" :key="item.id">
                  <div class="card border-0 box-shadow text-center h-100">
                    <img class="card-img-top" :src="item.imageUrl" alt="Card image cap">
                    <div class="card-body">
                      <h4 class="card-title">{{item.title}}</h4>
                      <p class="card-text">{{item.description}}</p>
                    </div>
                    <div class="card-footer border-top-0 bg-white">
                      <a v-if="item.is_enabled" href="#" class="btn btn-outline-secondary btn-block btn-sm" @click.prevent="getTheProduct(item.id)">
                        <i class="fa fa-cart-plus" aria-hidden="true"></i> 搶購去
                      </a>
                      <a v-else href="#" class="btn btn-outline-warning btn-block btn-sm disabled">
                        <i class="fas fa-exclamation-circle"></i> 缺貨中
                      </a>
                    </div>
                  </div>
              </div>
            </div>
            <div class="text-center pt-5 pb-5" v-else><h1>查無商品</h1></div>
            <!-- pagination -->
            <!-- <nav aria-label="Page navigation" class="my-5">
              <ul class="pagination justify-content-center">
                <li class="page-item disabled">
                  <a class="page-link" href="#" tabindex="-1">Previous</a>
                </li>
                <li class="page-item">
                  <a class="page-link" href="#">1</a>
                </li>
                <li class="page-item">
                  <a class="page-link" href="#">2</a>
                </li>
                <li class="page-item">
                  <a class="page-link" href="#">3</a>
                </li>
                <li class="page-item">
                  <a class="page-link" href="#">Next</a>
                </li>
              </ul>
            </nav> -->
            <page :pagedata=page @chosepage="chosepage"></page>
          </div>

        </div>
        <!-- tab-content end -->

      </div>

    </div>
  </div>
        <frontBkgBtn></frontBkgBtn>
    </div>
</template>


<script>

import frontBkgTop from "@/components/frontEnd/frontBkgTop";
import frontBkgBtn from "@/components/frontEnd/frontBkgBtn";
import page from "@/components/both/page";

export default {
    data(){
      return {
        isLoading:false,
        products:[],
        choosetype:"all",
        itemType:{
            all:[],
            suit:[],
            hat:[],
            shoes:[],
            others:[],
        },
        searchname:"",
        searchnameReady:"",
        page:{
          total_pages: "",
          current_page: 1,
          has_pre: "",
          has_next: "",
          onePageData:6,
        },
        cartList:[],
      };
    },
    methods:{
      getProduct(){
        const api=`${process.env.APIPATH}/api/${process.env.COUSTOMPATH}/products/all`;
        const vm=this;
        vm.isLoading=true;
        this.$http.get(api).then((response) => {
        console.log(response)
        vm.isLoading=false;

        vm.products = response.data.products;

        response.data.products.forEach(item => {
            if(item.category=="西裝"){
                vm.itemType.suit.push(item);
            }
            if(item.category=="帽子"){
                vm.itemType.hat.push(item);
            }
            if(item.category=="鞋子"){
                vm.itemType.shoes.push(item);
            }
            if(item.category=="配件"){
                vm.itemType.others.push(item);
            }
        });
        vm.itemType.all=vm.itemType.suit.concat(vm.itemType.hat,vm.itemType.shoes,vm.itemType.others)
        //按照分類排列
        })
      },
      getTheProduct(id){
        const api=`${process.env.APIPATH}/api/${process.env.COUSTOMPATH}/product/${id}`;
        const vm=this;
        vm.isLoading=true;
        this.$http.get(api).then((response) => {
        vm.isLoading=false;
        console.log("response",response)
        if(response.data.success){
            vm.$router.push(`/product/${response.data.product.id}`)
        }else{
            console.log("false message",response.data.message)
        }
        });
      },
      searchProducts(){
        const vm=this
        vm.searchname=vm.searchnameReady
        vm.searchnameReady=""
      },
      removeSearchName(){
        const vm=this
        vm.searchname=""
        vm.page.current_page=1

      },
      gerCart(){
        const api=`${process.env.APIPATH}/api/${process.env.COUSTOMPATH}/cart`;
        const vm=this;
        vm.isLoading=true;
        this.$http.get(api).then((response) => {
        console.log(response)
        vm.cartList=response.data.data.carts
        // this.$set(vm.data,cartList, response.data.data.carts)
        vm.isLoading=false;
        })
      },
      chosepage(page){
        const vm=this;
        vm.page.current_page=page
      }

    },
    created(){
        this.getProduct();
        this.gerCart()
    },
    computed:{
        productsType:function(){
            const vm=this
            let Result=[]
            if(this.choosetype=="all"){ Result = vm.itemType.all}
            if(this.choosetype=="suit"){ Result =  vm.itemType.suit}
            if(this.choosetype=="hat"){ Result =  vm.itemType.hat}
            if(this.choosetype=="shoes"){ Result =  vm.itemType.shoes}
            if(this.choosetype=="others"){ Result =  vm.itemType.others}


            if(vm.searchname==""){
              let pageDataStar=(vm.page.current_page-1)*vm.page.onePageData
              let pageDataEnd=pageDataStar+vm.page.onePageData
              
              vm.page.total_pages=Math.ceil(Result.length/vm.page.onePageData)

              if(vm.page.current_page==1){
                vm.page.has_pre= false
              }else{
                vm.page.has_pre= true
              }

              if(vm.page.total_pages>=2){
                if(vm.page.current_page>=vm.page.total_pages){
                  vm.page.has_next=false
                }else{
                  vm.page.has_next=true
                }
              }else{
                vm.page.has_next=false
              }

              console.log(Result)
              Result=Result.slice(pageDataStar, pageDataEnd)
              console.log(Result)
              return Result
            }else{
              let searchResult=[]
              Result.forEach(item => {
                if(item.title.search(vm.searchname)!=-1){
                    searchResult.push(item);
                }
              });

              vm.page.total_pages=Math.ceil(searchResult.length/vm.page.onePageData)
              vm.page.current_page=1
              vm.page.has_pre= false
              if(vm.page.total_pages>=2){
                vm.page.has_next=true
              }else{
                vm.page.has_next=false
              }

              return searchResult
            }
        },
    },
    components:{
        frontBkgTop,
        frontBkgBtn,
        page,
    },
}
</script>