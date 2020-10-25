<template>
  <div id="app">
    <div class="container">
      <Alert v-model="alert" message="Клиент успешно зарегестрирован" />
      <form action="#" autocomplete="off" class="form">
        <h1 class="form__title">Форма регистрации</h1>
        <SubForm v-for="(form, title) in forms" :title="title" :key="title">
          <component
            v-for="(input, key) in form"
            :id="key"
            :is="component(input.type)"
            :key="input.label"
            :type="input.type"
            v-bind="input.attrs"
            :errorMessage="input.errorMessage ? input.errorMessage : ''"
            :error="$v.forms[title][key].value.$error"
            :required="!!$v.forms[title][key].value.$params.required"
            v-model="$v.forms[title][key].value.$model"
            class="form__input"
          />
        </SubForm>
        <button class="form__button" type="submit" @click.prevent="submit">
          Отправить
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { required, numeric, minLength } from 'vuelidate/lib/validators'
const isPhone = value => {
  return /^7\d{10}$/.test(value)
} //phone valid
const errors = {
  required: 'Это поле обязательное',
  phone: 'Номер должен начинаться с 7 и быть длинной в 11 знаков'
}
export default {
  name: 'App',
  components: {
    SubForm: () => import('@/components/SubForm.vue'),
    Alert: () => import('@/components/Alert.vue'),
    TextField: () => import('@/components/TextField.vue'),
    Checkbox: () => import('@/components/Checkbox.vue'),
    Select: () => import('@/components/Select.vue')
  },
  data() {
    return {
      alert: false,
      forms: {
        ['Личная информация']: {
          firstName: {
            attrs: { label: 'Имя' },
            type: 'text',
            value: '',
            errorMessage: errors.required
          },
          lastName: {
            attrs: { label: 'Фамилия' },
            type: 'text',
            value: '',
            errorMessage: errors.required
          },
          patronymic: {
            attrs: { label: 'Отчество' },
            type: 'text',
            value: ''
          },
          birthday: {
            attrs: { label: 'День рождения' },
            type: 'text',
            value: '',
            errorMessage: errors.required
          },
          phone: {
            attrs: { label: 'Номер телефона' },
            type: 'number',
            value: '',
            errorMessage: errors.phone
          },
          sex: {
            attrs: { label: 'Пол' },
            type: 'text',
            value: ''
          },
          clientsGroup: {
            attrs: {
              label: 'Группа клиентов',
              options: ['VIP', 'Проблемные', 'ОМС'],
              multiple: true
            },
            type: 'select',
            value: [],
            errorMessage: errors.required
          },
          doctor: {
            attrs: {
              label: 'Лечащий врач',
              options: ['Иванов', 'Захаров', 'Чернышева']
            },
            type: 'select',
            value: []
          },
          sms: {
            attrs: { label: 'Не отправлять смс' },
            type: 'checkbox',
            value: false
          }
        },
        ['Адрес']: {
          index: {
            attrs: { label: 'Индекс' },
            type: 'number',
            value: ''
          },
          country: {
            attrs: { label: 'Страна' },
            type: 'text',
            value: ''
          },
          area: {
            attrs: { label: 'Область' },
            type: 'text',
            value: ''
          },
          city: {
            attrs: { label: 'Город' },
            type: 'text',
            value: '',
            errorMessage: errors.required
          },
          street: {
            attrs: { label: 'Улица' },
            type: 'text',
            value: ''
          },
          house: {
            attrs: { label: 'Дом' },
            type: 'text',
            value: ''
          }
        },

        ['Паспортные данные']: {
          type: {
            attrs: { label: 'Индекс' },
            type: 'number',
            value: '',
            errorMessage: errors.required
          },
          series: {
            attrs: { label: 'Серия' },
            type: 'text',
            value: ''
          },
          number: {
            attrs: { label: 'Номер' },
            type: 'number',
            value: ''
          },
          issuedBy: {
            attrs: { label: 'Кем выдан' },
            type: 'text',
            value: ''
          },
          date: {
            attrs: { label: 'Дата выдачи' },
            type: 'text',
            value: '',
            errorMessage: errors.required
          }
        }
      }
    }
  },
  validations: {
    forms: {
      ['Личная информация']: {
        lastName: { value: { required } },
        firstName: { value: { required } },
        patronymic: { value: {} },
        birthday: { value: { required } },
        phone: {
          value: {
            required,
            numeric,
            phoneValid: isPhone,
            minLength: minLength(11)
          }
        },
        sex: { value: {} },
        clientsGroup: { value: { required } },
        doctor: { value: {} },
        sms: { value: {} }
      },
      ['Адрес']: {
        index: { value: {} },
        country: { value: {} },
        area: { value: {} },
        city: { value: { required } },
        street: { value: {} },
        house: { value: {} }
      },
      ['Паспортные данные']: {
        type: { value: { required } },
        series: { value: {} },
        number: { value: {} },
        issuedBy: { value: {} },
        date: { value: { required } }
      }
    }
  },
  methods: {
    submit() {
      this.$v.forms.$touch()
      if (!this.$v.forms.$error) {
        this.alert = !this.alert
      }
    },
    component(type) {
      if (['text', 'number'].includes(type)) return 'TextField'
      if (['checkbox'].includes(type)) return 'Checkbox'
      if (['select'].includes(type)) return 'Select'
    }
  }
}
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {
  font-size: 10px;
}
.form {
  margin: 0 auto;
  max-width: 350px;
  padding: 40px 24px;
  border-radius: 4px;
  box-shadow: 0 20px 20px rgba(0, 0, 0, 0.2), 0px 0px 50px rgba(0, 0, 0, 0.2);
  &__title {
    font-size: 24px;
    margin-bottom: 20px;
  }
  &__input {
    margin-bottom: 10px;
  }
  &__button {
    font-size: 16px;
    border-radius: 4px;
    padding: 4px 8px;
    height: 40px;
    width: 100%;
    border: none;
    background-color: $yellow;
    transition: 0.3s;
    &:hover {
      background-color: rgba($yellow, 0.9);
    }
  }
}
.container {
  width: 1200px;
  padding: 15px;
  width: 100%;
}
</style>
