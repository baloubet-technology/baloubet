<template>
  <div>
    <div class="max-w-screen-xl mx-auto py-16 px-4 sm:px-6 lg:py-24 lg:px-8">
      <div class="max-w-3xl mx-auto text-center">
        <h1 class="text-3xl leading-9 font-extrabold text-gray-900 mb-4">{{ brand.name }}</h1>
        <div class="relative rounded-md shadow-sm">
          <div
            class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
          >
            <svg
              class="mr-3 h-4 w-4 text-gray-400"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
              <path
                fill-rule="evenodd"
                d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z"
                clip-rule="evenodd"
              />
            </svg>
          </div>
          <input
            id="search"
            class="form-input block w-full pl-9 sm:text-sm sm:leading-5"
            placeholder="Search"
          />
        </div>
        <p class="mt-4 text-lg leading-7 text-gray-500">Vous pouvez sélectionner une marque pour retrouver les produits disponibles à la vente par l'ensemble nos vendeurs sur Baloubet.</p>
      </div>
      <div class="pt-2 pb-6 md:py-6">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 md:px-8">
          <ul class="grid grid-cols-1 gap-6 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4">

            <li v-for="product in brand.product" v-if="product.status === 'Live'">
              <nuxt-link :to="{ name: 'products-id', params: { id: product.id }}">
                <div class="max-w-xs bg-white shadow-lg rounded-lg overflow-hidden my-10">
                  <div class="px-4 py-2">
                    <h1 class="text-gray-900 font-bold text-3xl uppercase">{{ limit(product.name, 35) }}</h1>
                    <p class="text-gray-600 text-sm mt-1">{{ limit(product.description, 90) }}</p>
                  </div>
                  <img class="h-56 w-full object-cover mt-2" src="https://images.unsplash.com/photo-1542291026-7eec264c27ff?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=750&q=80" alt="NIKE AIR">
                  <div class="flex items-center justify-between px-4 py-2 bg-gray-900">
                    <h1 class="text-gray-200 font-bold text-xl">{{ product.variant[0].price }} €</h1>
                    <button class="px-3 py-1 bg-gray-200 text-sm text-gray-900 font-semibold rounded">Add to card</button>
                  </div>
                </div>
              </nuxt-link>
            </li>

          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  data() {
    return {
      brand: {},
      errors: [],
    }
  },
  methods: {
    limit(str, length) {
      if (str.length < length) return str
      return `${str.substr(0, length)}...`
    },
  },
  apollo: {
    brand: {
      query: gql`
        query($id: ID!) {
          brand(id: $id) {
            id
            name
            product {
              id
              name
              description
              status
              variant {
                id
                sku
                price
                pictureUrl
              }
            }
          }
        }
      `,
      variables() {
        return { id: this.$route.params.eid }
      },
    },
  }
}
</script>
