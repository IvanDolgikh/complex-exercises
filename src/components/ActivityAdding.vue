<template>
  <div class="adding">
    <div class="adding__container">
      <h1 class="adding__title">Добавление упражнения в базу</h1>

      <div class="adding__form-container">
        <label class="adding__field">
          <img
            class="adding__image"
            v-if="imagePreview"
            :src="imagePreview"
            alt=""
          >
          <span
            v-else
            class="adding__image-default"
          >
            <span class="pi pi-camera"></span>
          </span>
          <input
            type="file"
            @change="onFileChange"
          >
        </label>

        <label class="adding__field">
          <span>Время:</span>
          <input
            type="text"
            v-model="time"
          >
        </label>

        <label class="adding__field">
          <span>Тип:</span>
          <input
            type="text"
            v-model="type"
          >
        </label>
      </div>


      <button
        class="adding__save-data"
        @click="emitData"
      >Добавить</button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  data() {
    return {
      imagePreview: '',
      time: '',
      type: '',
      id: 0,
    }
  },

  methods: {

    onFileChange(event: Event) {
      const target = event.target as HTMLInputElement;
      const file = target.files ? target.files[0] : null;
      if (!file) return

      const reader = new FileReader()
      reader.onload = (e) => {
        const base64 = e.target?.result as string
        this.imagePreview = base64
      }

      reader.readAsDataURL(file)
    },

    resetData() {
      alert('Упражнение добавлено')
      this.imagePreview = ''
      this.time = ''
      this.type = ''
    },

    emitData() {
      const data = {
        id: ++this.id,
        imagePreview: this.imagePreview,
        time: this.time,
        type: this.type,
      };
      this.$emit('saveData', data);
      this.resetData()
    }
  },
})
</script>

<style lang="scss">
.adding {

  &__container {
    display: flex;
    flex-direction: column;
    flex: 1;
    background-color: #ffffff;
    padding: 16px 24px;
    box-shadow: 4px 4px 8px 0px rgba(0, 0, 0, 0.2);
    border-radius: 6px;
    height: 100%;
  }


  &__form-container {
    display: grid;
    column-gap: 30px;
    grid-template-columns: min-content 1fr;
    margin-bottom: 24px;
    width: fit-content;
  }

  &__field {
    grid-column: 2;
    align-self: center;
    display: flex;
    justify-content: end;
    align-items: center;
    column-gap: 10px;
    height: fit-content;
    font-size: 18px;
    font-weight: 300;
  }

  &__field:has(input[type="file"]) {
    grid-column: 1;
    grid-row: 1 / 3;

    input {
      position: absolute;
      width: 1px;
      height: 1px;
      opacity: 0;
      overflow: hidden;
      z-index: -1;
    }
  }

  &__image {
    width: 120px;
    height: 120px;
    border-radius: 20%;
    object-fit: cover;
  }

  &__image-default {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 120px;
    height: 120px;
    background-color: #dbdbdb;
    border-radius: 20%;
    cursor: pointer;

    span {
      font-size: 30px;
      color: #5e5e5e;
    }
  }

  &__save-data {
    margin-top: auto
  }

}
</style>