<template>
 <div class="col-span-1 lg:col-span-4 order-first lg:order-last">
  <h4 class="text-3xl text-gray-700 mb-5">Order Summary</h4>
  <div class="p-10 rounded-md shadow-md bg-white">
   <TheItem :key="i" v-for="(item, i) in items" :item="item"  />
  </div>
 </div>
</template>

<script>
export default {
  props: {
    items: Object
  },
  data() {
    return {

    };
  }
};
</script>
