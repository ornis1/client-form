<template>
  <div class="subform">
    <h2 class="subform__title">{{ title }}:</h2>
    <div class="subform-group">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      required: true,
      type: String,
      default: ''
    }
  }
}
</script>

<style lang="scss">
.subform {
  &__title {
    text-align: left;
    font-size: 16px;
    margin-top: 0;
    margin-bottom: 10px;
  }
  padding-bottom: 30px;
}
</style>
