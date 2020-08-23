<template>
  <main class="container mx-auto flex flex-col items-center h-screen w-full">
    <h1 class="text-6xl font-semibold text-black mt-4">
      Prismic landing pages
    </h1>
    <section class="container mx-auto mt-4">
      <div v-for="landing in landings" :key="landing.uid" class="h-32 w-64 shadow-xl rounded-lg flex flex-col items-center justify-center">
        <h2 class="text-xl">
          {{ landing.data.meta_title }}
        </h2>
        <nuxt-link :to="landing.url">
          Visit landing
        </nuxt-link>
      </div>
    </section>
  </main>
</template>

<script>
export default {
  name: 'Index',
  async asyncData ({ $prismic, error }) {
    try {
      let landings
      await $prismic.api.query(
        $prismic.predicates.at('document.type', 'page'),
        { }
      ).then((res) => {
        landings = res.results
      })
      return {
        landings
      }
    } catch (e) {
      error({ statusCode: 404, message: 'Page not found' })
      // eslint-disable-next-line no-console
      console.err(e)
    }
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/

</style>
