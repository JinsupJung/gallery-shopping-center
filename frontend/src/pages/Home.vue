<template>
  <div class="home">
    <div class="album py-5 bg-light">
      <div class="container">
        <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3">
          <div class="col" v-for="(item, idx) in states.items" :key="idx">
          <Card :item="item"/>
<!--            {{ item}}-->
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
// import Card from "@/components/Card.vue";
import axios from "axios";
import {reactive} from "vue";
import Card from "@/components/Card.vue";

export default {
  name: "Home",
  components: {Card},
  setup() {
    const states = reactive({
      items: []
    })
    axios.get("/api/items").then((res)=>{
      console.log(res);
      states.items = res.data;
    })
    return { states };
  }
}
</script>

<style scoped>

</style>