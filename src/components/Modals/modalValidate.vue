<template>
  <Modal :title="title" @close="$emit('close')">
    <template v-slot:body>
      <form @submit.prevent="onSubmit">

        <!--        name-->
        <div class="form-item" :class="{errorInput: v$.name.$error}">
          <label>Имя:</label>
          <p class="errorText" v-if="v$.name.required.$invalid && v$.name.$error">Обязательное поле!</p>
          <p class="errorText" v-if="v$.name.minLength.$invalid && v$.name.$error">Длина должна быть не менее
            {{ v$.name.minLength.$params.min }} символов!</p>
          <input
            v-model="name"
            @change="v$.name.$touch()"
            :class="{error: v$.name.$error}"
          />
        </div>

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
            :class="{error: v$.password.$error && v$.confirmPassword.sameAs.$invalid}" />
          <label>Repeat password:</label>
          <p class="errorText" v-if="v$.confirmPassword.sameAs.$invalid">Пароли не совпадают!</p>
          <input
            v-model="confirmPassword"
            :class="{error: v$.confirmPassword.sameAs.$invalid}" />
        </div>

        <!--        button-->
        <button class="btn btnPrimary">Отправить</button>
      </form>
    </template>
  </Modal>
</template>

<script>
import { email, minLength, required, sameAs } from '@vuelidate/validators';
import { useVuelidate } from '@vuelidate/core';
import Modal from '@/components/Modals/UI/Modal';


export default {
	components: { Modal },

	data() {
		return {
			title: 'Форма с валидацией',
			email: '',
			name: '',
			password: '',
			confirmPassword: ''
		};
	},
	methods: {
		onSubmit() {
			// touch запускает валидацию
			this.v$.$touch();

			if (!this.v$.$invalid) {
				const user = {
					name: this.name,
					email: this.email,
					password: this.password
				};
				console.log(user);

				this.name = '';
				this.email = '';
				this.password = '';
				this.confirmPassword = '';
				this.v$.$reset();
				this.$emit('close');
			}
		}
	},
	validations() {
		return {
			name: {
				required,
				minLength: minLength(4)
			},
			email: {
				required,
				email
			},
			password: {
				required
			},
			confirmPassword: {
				sameAs: sameAs(this.password)
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

input.error {
  border-color: #ff0000;
}
</style>
