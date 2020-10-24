<template>
  <div class="text-field">
    <fieldset class="text-field__fieldset">
      <legend class="text-field__legend">
        {{ label + (required ? '*' : '') }}
      </legend>
      <label :class="classes" :for="id">
        <input
          class="text-field__input"
          :id="id"
          :type="type"
          v-bind="$attrs"
          :value="value"
          v-on="listeners"
          :inputmode="type"
        />
      </label>
      <slot name="error-message">
        <span v-show="error" :class="['text-field__error', { error: error }]">
          {{ errorMessage }}
        </span>
      </slot>
    </fieldset>
  </div>
</template>

<script>
export default {
  inheritAttrs: false,
  props: {
    errorMessage: {
      default: ''
    },
    value: {
      default: ''
    },
    type: {
      default: 'text'
    },
    label: {
      type: String,
      default: ''
    },
    error: {
      type: Boolean,
      default: false
    },
    required: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    id() {
      const { id } = this.$attrs
      return id ? id : new Date().getTime()
    },
    classes() {
      return ['text-field__label', { error: this.error }]
    },
    listeners() {
      return {
        ...this.$listeners,
        input: event => this.$emit('input', event.target.value)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
fieldset {
  border: none;
  text-align: left;
  line-height: 16px;
  color: #474d57;
}
legend {
  font-size: 1.2rem;
}

.error {
  color: $danger;
}
.text-field {
  &__fieldset {
    margin-bottom: 24px;
  }
  &__legend {
    color: #474d57;
    font-size: 12px;
    line-height: 16px;
  }
  &__input {
    height: 100%;
    width: 100%;
    color: #1e2026;
    font-size: 14px;
    border: none;
    outline: none;
    background: inherit;
  }
  &__label {
    transition: 0.2s;
    padding: 0 8px;
    height: 40px;
    margin-top: 4px;
    display: block;
    border: 1px solid black;
    border-radius: 4px;
    &.error {
      border-color: $danger !important;
    }
    &:hover:not(.error),
    &:active:not(.error),
    &:focus-within:not(.error) {
      border-color: $yellow;
    }
  }
  &__error {
    margin-top: 4px;
    font-size: 12px;
    line-height: 16px;
  }
}
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type='number'] {
  -moz-appearance: textfield;
}
</style>
