<template>
  <div class="cart">
    <div class="container">
        <ul>
          <li v-for="(item, idx) in state.items" :key="idx" >
<!--            <span class="img" :style="{backgroundImage: `url(${item.imgPath})`}"/>-->
            <img :src="item.imgPath"/>
            <span class="name">{{ item.name }}</span>
            <span class="price">{{ lib.getNumberFormatted(item.price - item.price * item.discountPer / 100) }}원</span>
            <i class="fa fa-trash" @click="remove(item.id)"></i>
          </li>
          <router-link to="/order" class="btn btn-primary">주문하기</router-link>

        </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import {reactive} from "vue";
import lib from "@/scripts/lib";


export default {
  name: "Cart",


  setup() {
    const state = reactive({
          items: []
        }
    )

    const load = () => {
      axios.get("/api/cart/items").then(({data}) => {
        console.log({data});
        state.items = data;
      })
    }


    const remove = (itemId) => {
      axios.delete(`/api/cart/items/${itemId}`).then(() => {
        console.log("delete success")
        load();
      })
    }

    load();

    return {state, lib, remove, load};
  }


}
</script>

<style scoped>
.cart ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
.cart ul li {
  border: 1px solid #eee;
  margin-top: 25px;
  margin-bottom: 25px;
}
.cart ul li img {
  width: 150px;
  height: 150px;
}
.cart ul li .name {
  margin-left: 25px;
}
.cart ul li .price {
  margin-left: 25px;
}
.cart ul li i {
  float: right;
  font-size: 20px;
  margin-top: 65px;
  margin-right: 50px;
}
.cart .btn {
  width:300px;
  display:block;
  margin:0 auto;
  padding:30px 50px;
  font-size: 20px;
}
</style>