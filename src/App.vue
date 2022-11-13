<template>

  <InputTag @onTagsChange="handleOnTagsChange" />

</template>


<script>

import InputTag from "./components/InputTag.vue"

export default {
  components: {
    InputTag,
  },
  methods: {
    handleOnTagsChange(tag) {
      console.log(tag)
    }
  }
}

</script>



<style scoped>

</style>