<template>
  <div class="flex justify-center">
    <div class="flex flex-col w-full p-8 text-gray-800 bg-white shadow-lg pin-r pin-y md:w-4/5 lg:w-4/5 my-8">
      <div class="flex-1">
        <table class="w-full text-sm lg:text-base" cellspacing="0">
          <thead>
            <tr class="h-12 uppercase">
              <th class="hidden md:table-cell"></th>
              <th class="text-left">Product</th>
              <th class="lg:text-right text-left pl-5 lg:pl-0">
                <span class="lg:hidden" title="Quantity">Qtd</span>
                <span class="hidden lg:inline">Quantity</span>
              </th>
              <th class="hidden text-right md:table-cell">Unit price</th>
              <th class="text-right">Total</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="dish in selectedDishes">
              <td class="hidden pb-4 md:table-cell">
                <a href="#">
                  <img :src="dish.picture" class="h-20 w-20 rounded" alt="Thumbnail">
                </a>
              </td>
              <td>
                <span class="text-sm lg:text-base font-medium">
                  {{ dish.name }}
                </span>
              </td>
              <td class="text-right">
                <span class="text-sm lg:text-base font-medium">
                  <a><span class="uk-badge" @click="removeFromCart(dish)">-</span></a>
                  {{ dish.quantity }}
                  <a class="uk-margin-left"><span class="uk-badge" @click="addToCart(dish)">+</span></a>
                </span>
              </td>
              <td class="hidden text-right md:table-cell">
                <span class="text-sm lg:text-base font-medium">
                  10.00€
                </span>
              </td>
              <td class="text-right">
                <span class="text-sm lg:text-base font-medium">
                  20.00€
                </span>
              </td>
            </tr>
          </tbody>
        </table>
        <hr class="pb-6 mt-6">

          <button type="button" class="px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
            Button text
          </button>

      </div>
    </div>
  </div>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  methods:{
    ...mapMutations({
      addToCart: 'cart/add',
      removeFromCart: 'cart/remove'
    })
  },
  computed: {
    id() {
      return this.$route.params.id
    },
    selectedDishes() {
      return this.$store.getters['cart/items']
    },
    price() {
      return this.$store.getters['cart/price']
    },
    numberOfItems() {
      return this.$store.getters['cart/numberOfItems']
    }
  }
}
</script>
