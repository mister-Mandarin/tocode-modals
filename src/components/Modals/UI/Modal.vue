<template>
  <transition name="modal">
    <div class="modal__wrapper" @click="$emit('close')">
      <div class="modal-content" @click.stop="">
        <div class="modal-header">
          <span class="modal-title"> {{ title }} </span>
          <span class="button-close" @click="$emit('close')">×</span>
        </div>
        <div class="modal-body">
          <slot name="body">default body</slot>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {};
  },
  mounted() {
    document.body.addEventListener('keyup', e => {
      if (e.key === 'Escape') {
        this.$emit('close');
      }
    });
  },
  computed: {},
  methods: {}
};
</script>
