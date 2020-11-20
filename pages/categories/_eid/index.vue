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
              <li v-for="product in tag.product" v-if="product.status === 'Live'">

                <div class="max-w-xs bg-white shadow-lg rounded-lg overflow-hidden">
                  <nuxt-link :to="{ name: 'products-id', params: { id: product.id }, query: { plan: 'private' }}">
                    <div class="px-4 py-2">
                      <p class="text-gray-900 font-bold uppercase">{{ limit(product.name, 18) }}</p>
                      <p class="text-gray-600 text-sm mt-1">{{ limit(product.description, 50) }}</p>
                    </div>
                    <img class="h-56 w-full object-cover mt-2" :src="product.variant[0].pictureUrl">
                  </nuxt-link>
                  <div class="flex items-center justify-between px-4 py-2 bg-gray-900">
                    <h1 class="text-gray-200 font-bold text-xl">{{ product.variant[0].price }} €</h1>
                    <button class="px-3 py-1 bg-gray-200 text-sm text-gray-900 font-semibold rounded">Add to card</button>
                  </div>
                </div>

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
                description
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
