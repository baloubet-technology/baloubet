<template>
  <VueIfBot>
    <CookieConsent
      data-cy="cookie-box"
      class="flex justify-center items-center text-light-onSurfaceSecondary dark:text-dark-onSurfaceSecondary border border-top border-light-border dark:border-dark-border flex-col pt-2 px-2 md:pt-0 md:px-0 md:flex-row text-center"
    >
      <template slot="message">
        <p>
          Nous utilisons des cookies pour l'analyse des utilisateurs et les améliorations de Baloubet!
          <a class="underline" href="https://www.cookiesandyou.com/">
            En savoir plus sur les cookies
          </a>
        </p>
      </template>
      <template slot="button">
        <button
          class="ml-4 no-underline mt-4 font-medium text-sm px-4 py-2 shadow uppercase rounded hover:shadow-md sm:mr-4 py-3 px-6 text-base mb-4 primary bg-black text-white hover:bg-black"
        >
          Accepter
        </button>
      </template>
    </CookieConsent>
  </VueIfBot>
</template>

<script>
import VueIfBot from 'vue-if-bot/dist/vue-if-bot.es'
import CookieConsent from 'vue-cookieconsent-component/src/components/CookieConsent.vue'
export default {
  components: {
    VueIfBot,
    CookieConsent
  }
}
</script>
