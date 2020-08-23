<template>
  <main>
    <slice-zone
      type="page"
      :uid="$route.params.uid"
    />
  </main>
</template>

<script>
import SliceZone from 'vue-slicezone'
export default {
  name: 'LandingPage',
  components: {
    SliceZone
  },
  async asyncData ({ $prismic, params, error }) {
    try {
      const page = (await $prismic.api.getByUID('page', params.uid)).data
      return {
        doc: page
      }
    } catch (e) {
      error({ statusCode: 404, message: 'Page not found' })
    }
  }

}
</script>
