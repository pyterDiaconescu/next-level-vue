<template>
  <div>
    <label v-if="label">{{ label }}</label>
    <input type="text" />
  </div>
</template>

<script>
export default {
  name: 'BaseInput',
  props: {
    label: {
      type: String,
      default: ''
    },
  },
}
</script>

<style scoped>

</style>