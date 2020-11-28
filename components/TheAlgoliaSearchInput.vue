<template>
  <div class="md:hidden container">
    <ais-instant-search-ssr>
      <div>
        <ais-search-box>
          <div slot-scope="{ currentRefinement, isSearchStalled, refine }">
            <input
              type="search"
              class="appearance-none min-w-0 w-full bg-white border border-gray-300 py-2 px-4 text-base leading-6 rounded-md text-gray-900 placeholder-gray-500 focus:outline-none focus:border-blue-300 focus:shadow-outline focus:placeholder-gray-400 transition duration-150 ease-in-out sm:max-w-xs"
              v-model="currentRefinement"
              @input="refine($event.currentTarget.value)"
            >
          </div>

          <div slot-scope="{ currentRefinement, isSearchStalled, refine }" class="relative z-0 flex-1 px-2 flex items-center justify-center sm:absolute sm:inset-0">
            <div class="max-w-s w-full">
              <label for="search" class="sr-only">Search</label>
              <div class="relative">
                <div class="pointer-events-none absolute inset-y-0 left-0 pl-3 flex items-center">
                  <!-- Heroicon name: search -->
                  <svg class="flex-shrink-0 h-5 w-5 text-gray-400" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                    <path fill-rule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z" clip-rule="evenodd" />
                  </svg>
                </div>
                <input v-model="currentRefinement" @input="refine($event.currentTarget.value)" name="search" id="search" class="block w-full bg-white border border-gray-300 rounded-md py-2 pl-10 pr-3 text-sm placeholder-gray-500 focus:outline-none focus:text-gray-900 focus:placeholder-gray-400 focus:ring-1 focus:ring-gray-900 focus:border-gray-900 sm:text-sm" placeholder="Search" type="search">
              </div>
            </div>
          </div>

        </ais-search-box>
      </div>

      <ais-state-results>
        <div slot-scope="{ query }">
          <div v-if="query.length">




            <div class="absolute left-1/2 transform -translate-x-1/2 mt-3 px-2 w-screen max-w-xs sm:px-0">
              <div class="rounded-lg shadow-lg ring-1 ring-black ring-opacity-5 overflow-hidden">
                <div class="z-20 relative grid gap-6 bg-white px-5 py-6 sm:gap-8 sm:p-8">

                  <ais-hits>

                    <div slot="item" slot-scope="{ item }" class="-m-3 p-3 block rounded-md hover:bg-gray-50 transition ease-in-out duration-150">
                      <div class="flex items-center">
                        <img :src="item.picture" class="h-8 w-8 rounded-md border-gray-300">
                        <p class="text-base font-medium text-gray-900 ml-3">
                          {{ limit(item.name, 20) }}
                        </p>
                      </div>
                    </div>

                  </ais-hits>

                </div>
              </div>
            </div>



          </div>
        </div>
      </ais-state-results>
    </ais-instant-search-ssr>
  </div>
</template>

<script>
  import {
    AisInstantSearchSsr,
    AisHits,
    AisSearchBox,
    AisStateResults,
    AisStats,
    AisPoweredBy,
    AisPagination,
    createServerRootMixin,
  } from 'vue-instantsearch'

  import algoliasearch from 'algoliasearch/lite';

  const searchClient = algoliasearch(
    'OQKR7DERMO',
    '12cfb689e4a92ed7faba3e457be97e2b'
  );

  export default {
    components: {
      AisInstantSearchSsr,
      AisHits,
      AisSearchBox,
      AisStateResults,
      AisStats,
      AisPoweredBy,
      AisPagination,
      createServerRootMixin,
    },
    mixins: [
      createServerRootMixin({
        searchClient,
        indexName: 'products',
      }),
    ],
    data () {
      return {
        instantSearchState: null,

        aisPaginationClassNames: {
              'ais-Pagination': 'mb-10',
              'ais-Pagination-list': 'flex flex-wrap',
              'ais-Pagination-link': 'inline-block w-10 h-10 flex items-center justify-center',
              'ais-Pagination-item': 'bg-white rounded mr-3 mb-2',
              'ais-Pagination-item--selected': 'bg-blue-500 text-white'
            }
      }
    },

    provide () {
      return {
        $_ais: this.instantsearch
      }
    },

    beforeMount () {
      this.instantsearch.hydrate(this.instantSearchState)
    },

    asyncData ({ app, query }) {
      const instantsearch = app.instantsearch

      return instantsearch
        .findResultsState({
          query: query.query,
          hitsPerPage: 10
        })
        .then(() => {
          return {
            instantSearchState: instantsearch.getState()
          }
        })
    },
    methods: {
    limit(str, length) {
      if (str.length < length) return str
      return `${str.substr(0, length)}...`
    }
  },
  }
</script>
