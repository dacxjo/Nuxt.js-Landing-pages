<template>
  <section class="section bg-cover h-screen grid gap-10 grid-cols-2" :style="{ backgroundImage: `url(${slice.primary.background_image.url})` }">
    <div>
      <prismic-rich-text v-if="slice.primary.title" class="text-6xl text-left text-white" :field="slice.primary.title" />

      <prismic-rich-text v-if="slice.primary.landing_desc" class="text-3xl mt-10 text-left text-white" :field="slice.primary.landing_desc" />
    </div>

    <div class="p-4 bg-white rounded justify-start items-center flex flex-col">
      <h2 class="text-2xl">
        {{ slice.primary.title[0].text }}
      </h2>
      <prismic-rich-text v-if="slice.primary.landing_desc" class="text-lg mt-10 text-center text-black" :field="slice.primary.landing_desc" />
      <form v-if="formFetched" class="w-full" action="">
        <div v-for="field in formFields" :key="field.name[0].text" class="p-2">
          <input v-if="field.type !== 'checkbox'" :placeholder="field.placeholder[0].text" class="border w-full border-gray-500 rounded-full px-3 h-10" :type="field.type">
          <div v-else class="flex flex-row-reverse flex-end items-center">
            <label for="">{{ field.label[0].text }}</label>
            <input :type="field.type">
          </div>
        </div>
        <input class="w-full bg-green-500 text-white rounded-full h-10" type="submit" :value="form.data.submit_button_text[0].text">
      </form>
    </div>
  </section>
</template>
<script>
export default {
  // eslint-disable-next-line vue/require-prop-types
  name: 'HeroSectionSlice',
  props: ['slice'],
  data () {
    return {
      form: null,
      formFetched: false
    }
  },
  computed: {
    endpoint () {
      return this.$prismic && this.$prismic.apiEndpoint
        ? this.$prismic.apiEndpoint.split('/').slice(0, -2).join('/').replace('cdn.', '')
        : null
    },
    formFields () {
      if (this.form.data) {
        return this.form.data.body[0].items
      }

      return null
    }
  },
  mounted () {
    this.form = this.getTheForm()
  },
  methods: {
    async getTheForm () {
      return await (this.$prismic.api.getByUID('forms', this.slice.primary.linked_form.uid))
        .then((res) => {
          this.form = res
          this.formFetched = true
          console.log(this.formFields)
        })
    }
  }
}
</script>
<style scoped>
.section {
  position: relative;
  color: #111;
  padding: 4em;
  text-align: center;
}
a {
  color: #111;
}
</style>
