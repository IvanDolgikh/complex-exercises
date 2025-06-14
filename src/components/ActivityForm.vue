<template>
  <div class="activity">
    <div class="activity__container">
      <h1>Количество упражнений</h1>

      <label class="activity__field">
        <span>1 тип</span>
        <input
          type="text"
          v-model="firstType"
        >
      </label>

      <label class="activity__field">
        <span>2 тип</span>
        <input
          type="text"
          v-model="secondType"
        >
      </label>

      <label class="activity__field">
        <span>3 тип</span>
        <input
          type="text"
          v-model="thirdType"
        >
      </label>

      <label class="activity__field">
        <span>Время на изучение упражнения:</span>
        <input
          type="text"
          v-model="time"
        >
      </label>

      <button
        class="activity__start-button"
        @click="sendInfoComplex"
      >Старт</button>
    </div>
  </div>
</template>


<script lang="ts">
export default {
  props: ["arrayExercises", "timeForExercise"],

  data() {
    return {
      firstType: null,
      secondType: null,
      thirdType: null,
      time: null,
    }
  },

  methods: {

    sendInfoComplex() {
      const infoComplex = {
        firstType: this.firstType,
        secondType: this.secondType,
        thirdType: this.thirdType,
        time: this.time,
      }

      this.$emit('sendInfoComplex', infoComplex)
    },
  }
}
</script>

<style lang="scss">
.activity {


  &__container {
    background-color: #ffffff;
    padding: 16px 24px;
    box-shadow: 4px 4px 8px 0px rgba(0, 0, 0, 0.2);
    display: flex;
    flex-direction: column;
    row-gap: 12px;
    border-radius: 6px;
    border: none;
  }

  &__field {
    display: flex;
    align-items: center;
    display: flex;
    justify-content: start;
    align-items: center;
    column-gap: 10px;
    height: fit-content;
    font-size: 18px;
    font-weight: 300;
  }

  &__start-button {
    padding: 6px 24px;
  }
}
</style>