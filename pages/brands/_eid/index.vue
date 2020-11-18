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

            <li v-for="product in brand.product" v-if="product.status === 'Live'" class="col-span-1 flex flex-col text-center bg-white shadow border-solid border border-gray-100">
              <nuxt-link :to="{ name: 'products-id', params: { id: product.id }}">
                <img class="p-6" :src="product.variant[0].pictureUrl">
                <div class="-mt-px flex">
                  <div class="w-0 flex-1 flex">
                    <div class="relative -mr-px w-0 flex-1 inline-flex items-center justify-center py-2 text-sm leading-5 text-gray-700 font-medium border border-transparent rounded-bl-lg rounded-br-lg hover:text-gray-500 focus:outline-none focus:shadow-outline-blue focus:border-blue-300 focus:z-10 transition ease-in-out duration-150">
                      <div>
                        <p class="mt-1 text-sm leading-5 text-gray-500">{{ limit(product.name, 35) }}</p>
                        <p class="text-sm py-2 leading-5 font-medium text-black">{{ product.variant[0].price }} €</p>
                      </div>
                    </div>
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
        return { id: this.$route.params.id }
      },
    },
  }
}
</script>
