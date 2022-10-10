<template>
  <div class="md-body">

    <!--    простое модальное окно-->
    <Modals v-show="showFirstModal" :title="firstModalTitle" @close="closeFirst">
      <template v-slot:body>
        <p>
          Далеко-далеко за словесными горами в стране гласных и согласных живут
          рыбные тексты.
        </p>
        <button class="btn btnPrimary" @click="showFirstModal = false">
          Close
        </button>
      </template>
    </Modals>
    <button class="btn btnPrimary" @click="showFirstModal = true">
      {{ firstModalTitle }}
    </button>

    <!--    второе модальное окно с формой-->
    <Modals v-show="secondModal.show" :title="secondModal.title" @close="secondModal.show=false">
      <template v-slot:body>
        <form @submit.prevent="submitSecondForm">
          <label>Имя:</label>
          <input type="text" required v-model="secondModal.name">

          <label>Email:</label>
          <input type="email" required v-model="secondModal.email">
          <button class="btn btnPrimary">
            Отправить
          </button>
        </form>
      </template>
    </Modals>
    <button class="btn btnPrimary" @click="secondModal.show = true">
      Модальное окно с формой
    </button>

    <!--    третее модальное окно с формой и валидацией-->
    <button class="btn btnPrimary" @click="modalValidate = true">
      Модальное окно с формой + валидация
    </button>
    <modalValidate v-show="modalValidate" @close="modalValidate = false" />
  </div>
</template>

<script>
import Modals from '@/components/Modals/UI/Modal';
import modalValidate from '@/components/Modals/modalValidate';

export default {
	components: {
		Modals, modalValidate
	},
	data() {
		return {
			firstModalTitle: 'Простое модальное окно',
			showFirstModal: false,
			secondModal: {
				title: 'Форма',
				show: false,
				name: '',
				email: ''
			},
			modalValidate: false
		};
	},
	methods: {
		submitSecondForm() {
			console.log({
				name: this.secondModal.name,
				email: this.secondModal.email
			}
			);
			this.secondModal.name = '';
			this.secondModal.email = '';
			this.secondModal.show = false;
		},
		closeFirst() {
			this.showFirstModal = !this.showFirstModal;
		}
	}
};
</script>
