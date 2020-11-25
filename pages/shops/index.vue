<template>
  <ais-instant-search-ssr>
    <div>
      <div class="max-w-7xl mx-auto py-16 px-4 sm:px-6 lg:py-24 lg:px-8 lg:grid lg:grid-cols-3 lg:gap-x-8">
        <div>
          <h2 class="text-base font-semibold text-indigo-600 uppercase tracking-wide">Everything you need</h2>
          <p class="mt-2 text-3xl font-extrabold text-gray-900">Brand</p>
          <p class="mt-4 text-lg text-gray-500">Ac euismod vel sit maecenas id pellentesque eu sed consectetur. Malesuada adipiscing sagittis vel nulla nec.</p>
          <div class="mt-8">
            <ais-refinement-list attribute="brand" />
          </div>
        </div>
        <div class="mt-12 lg:mt-0 lg:col-span-2">
          <div class="max-w-7xl mx-auto px-4 sm:px-6 md:px-8">
            <ais-hits>
              <ul slot-scope="{ items }" class="grid grid-cols-1 gap-6 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-3">


                <li v-for="item in items" :key="item.objectID">

                  <div class="max-w-xs bg-white shadow-lg rounded-lg overflow-hidden">

                      <div class="px-4 py-2">
                        <p class="text-gray-900 font-bold uppercase">{{ limit(item.name, 18) }}</p>
                        <p class="text-gray-600 text-sm mt-1">{{ limit(item.description, 50) }}</p>
                      </div>
                      <img class="h-56 w-full object-cover mt-2" :src="item.picture">

                    <div class="flex items-center justify-between px-4 py-2 bg-gray-900">
                      <h1 class="text-gray-200 font-bold text-xl">50.98 €</h1>
                      <button class="px-3 py-1 bg-gray-200 text-sm text-gray-900 font-semibold rounded">Add to card</button>
                    </div>
                  </div>

                </li>


              </ul>
            </ais-hits>
          </div>
        </div>
      </div>
    </div>
    <ais-pagination />
  </ais-instant-search-ssr>
</template>

<script>
import {
  AisInstantSearchSsr,
  AisRefinementList,
  AisHits,
  AisHighlight,
  AisSearchBox,
  AisStats,
  AisPagination,
  createServerRootMixin,
} from 'vue-instantsearch';

import algoliasearch from 'algoliasearch/lite';

const searchClient = algoliasearch(
  'OQKR7DERMO',
  '12cfb689e4a92ed7faba3e457be97e2b'
);

export default {
  mixins: [
    createServerRootMixin({
      searchClient,
      indexName: 'products',
    }),
  ],
  serverPrefetch() {
    return this.instantsearch.findResultsState(this).then(algoliaState => {
      this.$ssrContext.nuxt.algoliaState = algoliaState;
    });
  },
  beforeMount() {
    const results =
      this.$nuxt.context.nuxtState.algoliaState || window.__NUXT__.algoliaState;

    this.instantsearch.hydrate(results);
  },
  components: {
    AisInstantSearchSsr,
    AisRefinementList,
    AisHits,
    AisHighlight,
    AisSearchBox,
    AisStats,
    AisPagination,
  },
  methods: {
    limit(str, length) {
      if (str.length < length) return str
      return `${str.substr(0, length)}...`
    }
  },
};
</script>
