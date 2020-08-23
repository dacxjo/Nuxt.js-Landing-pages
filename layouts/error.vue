<template>
  <div class="container mx-auto flex flex-col h-screen items-center w-full justify-center">
    <h1 v-if="error.statusCode === 404">
      Page not found
    </h1>
    <h1 v-else>
      An error occurred
    </h1>
    <nuxt-link class="p-2 rounded bg-green-400 text-white" to="/">
      Home page
    </nuxt-link>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/require-prop-types
  props: ['error']

}
</script>
