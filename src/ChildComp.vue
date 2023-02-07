<script>
export default {
  props: {
    msg: String
  },
  emits: ['response'],
  created() {
    // emit with argument
    this.$emit('response', 'hello from child')
  }
}
</script>

<template>
    <!-- <h2>A Child Component!  {{  msg || 'No props passed yet'  }}</h2> -->
    <slot>Fallback content</slot>
  </template>