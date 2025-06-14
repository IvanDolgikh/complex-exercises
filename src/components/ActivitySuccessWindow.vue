<template>
  <div class="success-window">
    <div class="success-window__container">
      <button
        @click="closeWindow"
        class="success-window__btn-close"
      >
        <span class="pi pi-times"></span>
      </button>
      <p>Упражнение добавлено!</p>
      <button
        @click="closeWindow"
        class="success-window__btn-ok"
      >ОК</button>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  props: ["isSuccessAdding"],

  methods: {
    closeWindow() {
      this.$emit('closeWindow')
    }
  }
}
</script>

<style lang="scss">
.success-window {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.201);
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;

  p {
    font-size: 18px;
  }

  &__container {
    width: 400px;
    height: 170px;
    background-color: #ebebeb;
    border-radius: 10px;
    padding: 20px;
  }

  &__btn-close {
    background-color: transparent;
    display: block;
    margin-left: auto;

    span {
      font-size: 18px;
    }
  }

  &__btn-ok {
    display: block;
    margin-left: auto;
  }
}
</style>