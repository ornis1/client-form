<template>
  <label class="check option">
    <input
      class="check__input"
      type="checkbox"
      :checked="value"
      v-on="listeners"
    />
    <svg class="check__box" viewBox="0 0 20 20">
      <rect class="check__focus" width="20" height="20" rx="4" />
      <rect class="check__square" x="3" y="3" width="14" height="14" rx="2" />
      <polyline class="check__mark" points="5 9 9 13 15 6" />
    </svg>
    {{ label }}
  </label>
</template>

<script>
export default {
  inheritAttrs: false,
  props: {
    label: {
      type: String,
      default: ''
    },
    value: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    id() {
      const { id } = this.$attrs
      return id ? id : new Date().getTime()
    },
    listeners() {
      return {
        ...this.$listeners,
        input: e => {
          this.$emit('input', e.target.checked)
        }
      }
    }
  }
}
</script>

<style lang="scss">
/* Check */
.option {
  display: flex;
  align-items: center;
  font-size: 14px;
  cursor: pointer;
}
.check {
  padding-left: 1.4em;
  &__input {
    position: absolute;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
  }
  &__box {
    position: absolute;
    margin-left: -1.4em;
    width: 1.2em;
    height: 1.2em;
  }
  &__focus {
    fill: none;
  }
  &__square {
    fill: #ffffff;
    stroke: black;
    stroke-width: 2px;
  }
  &__mark {
    fill: none;
    stroke: #ffffff;
    stroke-width: 3px;
  }
}

.check__input:hover + * .check__square {
  stroke: $yellow;
}
/* Checked */
.check__input:checked + * .check__square {
  fill: $yellow;
  stroke: $yellow;
}
/* Focused */
.check__input:focus-visible + * .check__focus {
  fill: $yellow;
}
/* Disabled */
.check__input:disabled + * .check__square {
  stroke: #9b9b9b;
}
.check__input:checked:disabled + * .check__square {
  fill: #9b9b9b;
}
</style>
