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
                      <div>
                        <img :src="item.picture" class="h-6 w-6 rounded-full">
                        <p class="text-base font-medium text-gray-900">
                          {{ item.name }}
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
    }
  }
</script>
