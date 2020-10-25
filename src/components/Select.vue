<template>
  <div class="wrapper">
    <label class="label" for="select">
      {{ $attrs.label }}
    </label>
    <select
      v-bind="$attrs"
      v-model="selected"
      @change="updateValue"
      name="select"
    >
      <option
        v-for="(option, index) in $attrs.options"
        :key="index"
        :value="option"
      >
        {{ option }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  inheritAttrs: false,
  props: {
    value: {
      type: [Array, String, Number]
    }
  },
  model: {
    event: 'change'
  },
  data() {
    return {
      selected: this.value
    }
  },
  methods: {
    updateValue() {
      this.$emit('change', this.selected)
    }
  },
  computed: {
    id() {
      const { id } = this.$attrs
      return id ? id : new Date().getTime()
    },
    listeners() {
      return {
        ...this.$listeners
      }
    }
  }
}
</script>

<style lang="scss" scoped>
select {
  margin-top: 4px;
  display: flex;
  width: 100%;
  height: 40px;
  color: black;
  font-size: 16px;
  border-radius: 4px;
  background-color: transparent;
  border: 1px solid black;
  &:focus {
    outline: none;
  }
  &:hover,
  &:active,
  &:focus {
    border-color: $yellow;
  }
}
option {
  background-color: transparent;
}
.label {
  display: block;
  font-size: 12px;
  color: #474d57;
  text-align: left;
}
</style>
