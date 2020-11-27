<template>
  <div class="uk-card uk-card-default uk-card-body uk-margin" uk-sticky="offset: 20; bottom: true">

    <div>

      <table class="uk-table uk-table-striped uk-table-small uk-table-responsive">
          <thead>
              <tr>
                  <th>Name</th>
                  <th>Price (unit)</th>
                  <th>Quantity</th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="dish in selectedDishes">
                  <td class="uk-width-1-2">{{ dish.name }}</td>

                  <td class="uk-table-shrink">{{ dish.quantity }}</td>
                  <td>
                    <a class="uk-margin-left"><span class="uk-badge" @click="addToCart(dish)">+</span></a>
                    <a><span class="uk-badge" style="background: #f0506e;" @click="removeFromCart(dish)">-</span></a>
                  </td>

              </tr>
          </tbody>
      </table>


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
