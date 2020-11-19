<template>
  <div>
    <div class="max-w-7xl mx-auto py-16 px-4 sm:px-6 lg:py-24 lg:px-8 lg:grid lg:grid-cols-3 lg:gap-x-8">
      <div>
        <h2 class="text-base font-semibold text-indigo-600 uppercase tracking-wide">Everything you need</h2>
        <p class="mt-2 text-3xl font-extrabold text-gray-900">{{ category.name }}</p>
        <p class="mt-4 text-lg text-gray-500">Ac euismod vel sit maecenas id pellentesque eu sed consectetur. Malesuada adipiscing sagittis vel nulla nec.</p>
        <div class="mt-8">
          <ul>
            <li v-for="tag in category.tag">
              <nuxt-link :to="{ name: 'categories-eid-tags-id', params: { id: tag.id }}">
                <p class="mt-4 text-lg text-gray-500">{{ tag.name }}</p>
              </nuxt-link>
            </li>
          </ul>
        </div>
      </div>
      <div class="mt-12 lg:mt-0 lg:col-span-2">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 md:px-8">
          <ul class="grid grid-cols-1 gap-6 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-3">
            <template v-for="tag in category.tag">
              <li v-for="product in tag.product" v-if="product.status === 'Live'" class="col-span-1 flex flex-col text-center bg-white shadow border-solid border border-gray-100">
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
            </template>
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
      category: {},
      errors: [],
    }
  },
  methods: {
    limit(str, length) {
      if (str.length < length) return str
      return `${str.substr(0, length)}...`
    }
  },
  apollo: {
    category: {
      query: gql`
        query($id: ID!) {
          category(id: $id) {
            id
            name
            tag {
              id
              name
              product {
                id
                name
                status
                variant {
                  id
                  price
                  pictureUrl
                }
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
