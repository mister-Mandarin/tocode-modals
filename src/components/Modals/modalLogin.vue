<template>
  <Modal :title="title" @close="$emit('close')">
    <template v-slot:body>
      <form @submit.prevent="onSubmit">

        <!--        email-->
        <div class="form-item" :class="{errorInput: v$.email.$error}">
          <label>Email:</label>
          <p class="errorText" v-if="v$.email.required.$invalid && v$.email.$error">Обязательное поле!</p>
          <p class="errorText" v-if="v$.email.email.$invalid && v$.email.$error">Некорректный email!</p>
          <input
            v-model="email"
            @change="v$.email.$touch()"
            :class="{error: v$.email.$error}" />
        </div>

        <!--        password-->
        <div class="form-item" :class="{errorInput: v$.password.$error}">
          <label>Password:</label>
          <p class="errorText" v-if="v$.password.$error">Обязательное поле!</p>
          <input
            v-model="password"
            @change="v$.password.$touch()"
            :class="{error: v$.password.$error}" />
        </div>

        <!--        button-->
        <button class="btn btnPrimary">Войти</button>
      </form>
      <div style="padding: 20px 0 20px 0; background-color: #edeaea">
        <span class="text-register" @click="$emit('toRegister')">Регистрация нового аккаунта</span>
      </div>
    </template>
  </Modal>
</template>

<script>
import { email, required } from '@vuelidate/validators';
import { useVuelidate } from '@vuelidate/core';
import Modal from '@/components/Modals/UI/Modal';


export default {
  components: { Modal },

  data() {
    return {
      title: 'Авторизация',
      email: '',
      password: ''
    };
  },
  methods: {
    onSubmit() {
      // touch запускает валидацию
      this.v$.$touch();

      if (!this.v$.$invalid) {
        const user = {
          email: this.email,
          password: this.password
        };
        console.log(user);

        this.email = '';
        this.password = '';
        this.v$.$reset();
        this.$emit('close');
      }
    }
  },
  validations() {
    return {
      email: {
        required,
        email
      },
      password: {
        required
      }
    };
  },
  setup: () => ({ v$: useVuelidate() })

};
</script>

<style lang="scss">
.form-item .errorText {
  display: block;
  margin-bottom: 8px;
  font-size: 14px;
  color: #ff0000;
}

.text-register {
  cursor: pointer;
  text-decoration: underline;
  font-size: 14px;
}

input.error {
  border-color: #ff0000;
}
</style>
