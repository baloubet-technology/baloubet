<template>
  <IfBot>
    <CookieConsent
      message="Do you likes coockies? 🍪 We use coockies to ensure you get the best experience on our website."
    />
  </IfBot>
</template>

<script>
import IfBot from 'vue-if-bot/dist/vue-if-bot.es'
import CookieConsent from 'vue-cookieconsent-component/src/components/CookieConsent.vue'
export default {
  components: {
    IfBot,
    CookieConsent
  }
}
</script>

<style>
.cookie-consent {
  display: flex;
  padding: 10px;
  align-items: center;
  align-self: center;
  justify-content: center;
  color: black;
}
.cookie-consent button {
  border: 1px solid black;
  padding: 10px;
  margin-left: 20px;
  min-width: 140px;
}
</style>
