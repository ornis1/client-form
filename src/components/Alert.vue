<template>
  <transition name="fade">
    <div class="alert" v-show="value">
      <div class="alert-content">
        <div class="alert__icon-wrapper">
          <object class="alert__icon" type="image/svg+xml" :data="successIcon">
            <img :src="successIcon" alt="" />
          </object>
        </div>
        <div class="alert__message">
          {{ message }}
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import successIcon from '@/assets/success.svg'
export default {
  name: 'Alert',
  data() {
    return {
      successIcon
    }
  },
  props: {
    value: {
      type: Boolean,
      default: false
    },
    message: {
      type: String,
      default: ''
    },
    delay: {
      type: Number,
      default: 2000
    }
  },
  computed: {
    model: {
      get() {
        return this.value
      },
      set(v) {
        this.$emit('input', v)
      }
    }
  },
  watch: {
    model(v) {
      if (v) {
        setTimeout(() => {
          this.model = false
        }, this.delay)
      }
    }
  }
}
</script>

<style lang="scss">
.alert {
  position: fixed;
  top: 20px;
  left: 0;
  right: 0;
  &-content {
    background-color: $success;
    color: white;
    padding: 16px;
    border-radius: 10px;
    display: inline-flex;
    justify-content: center;
    align-items: center;
    font-size: 16px;
  }
  &__icon {
    display: flex;
    margin-right: 10px;
  }
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
