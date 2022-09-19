<template>
  <Modals :title="title" @close="$emit('close')">
    <template v-slot:body>
      <form @submit.prevent="">
        <label>Имя:</label>
        <input type="text" required>

        <label>Email:</label>
        <input type="email" required>
        <button class="btn btnPrimary">
          Отправить
        </button>
      </form>
    </template>
  </Modals>
</template>

<script>
import Modals from '@/components/Modals/UI/Modal';

export default {
	components: {Modals},
	data() {
		return {
			title: 'Форма с валидацией'
		};
	}
};

</script>
