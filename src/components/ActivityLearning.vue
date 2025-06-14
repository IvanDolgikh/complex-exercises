<template>
  <div class="learning">
    <div class="learning__container">
      <div
        class="learning__image-container"
        v-if="data.currentStage === 1 && getExerciseFirstType.length !== 0 && data.isFilled"
      >
        <img
          class="learning__image"
          v-for="exercise in getExerciseFirstType"
          :key="exercise.id"
          :src="exercise.imagePreview"
          alt=""
        >
      </div>

      <div
        class="learning__image-container"
        v-else-if="data.currentStage === 2 && getExerciseSecondType.length !== 0 && data.isFilled"
      >
        <img
          class="learning__image"
          v-for="exercise in getExerciseSecondType"
          :key="exercise.id"
          :src="exercise.imagePreview"
          alt=""
        >
      </div>

      <div
        class="learning__image-container"
        v-else-if="data.currentStage === 3 && getExerciseThirdType.length !== 0 && data.isFilled"
      >
        <img
          class="learning__image"
          v-for="exercise in getExerciseThirdType"
          :key="exercise.id"
          :src="exercise.imagePreview"
          alt=""
        >
      </div>

      <div
        v-else
        class="learning__image-preview"
      >
        <span class="pi pi-camera"></span>
      </div>

      <p class="learning__time-learning">Время на изучение: <span>{{ currentTime }} сек.</span></p>
    </div>
  </div>
</template>

<script lang="ts">

interface DataItem {
  id: number
  imagePreview: string
  time: string
  type: string
}

export default {
  props: ["data"],


  data() {
    return {
      currentTime: 0,
      timer: null as number | null,
    }
  },

  methods: {

    startTimer() {
      this.currentTime = this.data.infoComplex.timeForExercise
      this.timer = setInterval(() => {
        this.currentTime--;


        if (this.currentTime <= 0) {

          const exercises = this.getCurrentExercises();


          if (this.data.currentStage < 3) {
            this.resetTimer();
          } else {
            this.stopTimer()
          }

          this.$emit('transferCurrentExercise', exercises)
          this.stopTimer()
        }
      }, 1000);

    },

    getCurrentExercises() {
      switch (this.data.currentStage) {
        case 1: return this.getExerciseFirstType;
        case 2: return this.getExerciseSecondType;
        case 3: return this.getExerciseThirdType;
        default: return [];
      }
    },

    resetTimer() {
      this.stopTimer();
      this.currentTime = this.data.infoComplex.timeForExercise;
      this.startTimer();
    },

    stopTimer() {
      if (this.timer) {
        clearInterval(this.timer);
        this.timer = null;
      }
    },
  },

  computed: {
    getExerciseFirstType() {
      const filteredTypes = this.data.arrayExercises.filter((item: DataItem) => {
        return item.type === '1'
      });

      return filteredTypes
        .sort(() => 0.5 - Math.random())
        .slice(0, this.data.infoComplex.firstTypeAmount)
    },

    getExerciseSecondType() {
      const filteredTypes = this.data.arrayExercises.filter((item: DataItem) => {
        return item.type === '2'
      });

      return filteredTypes
        .sort(() => 0.5 - Math.random())
        .slice(0, this.data.infoComplex.secondTypeAmount)
    },

    getExerciseThirdType() {
      const filteredTypes = this.data.arrayExercises.filter((item: DataItem) => {
        return item.type === '3'
      });

      return filteredTypes
        .sort(() => 0.5 - Math.random())
        .slice(0, this.data.infoComplex.thirdTypeAmount)
    },
  },

  watch: {
    'data.isFilled': {
      handler(newValue) {
        if (newValue) {
          this.resetTimer()
        }
      },
      immediate: true
    },
  },
}
</script>

<style lang="scss">
.learning {


  &__container {
    background-color: #f7f7f7;
    padding: 16px 24px;
    box-shadow: 0px 0px 8px 0px rgba(0, 0, 0, 0.2);
    border-radius: 6px;
  }

  &__image-container {
    display: flex;
    justify-content: center;
    column-gap: 20px;
  }

  &__image {
    width: 350px;
    height: 250px;
    object-fit: cover;
    border-radius: 14px;
  }

  &__image-preview,
  &__image {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 350px;
    height: 250px;
    background-color: #dbdbdb;
    border-radius: 20px;

    span {
      font-size: 46px;
      color: #5e5e5e;
    }
  }

  &__time-learning {
    font-size: 18px;
    width: fit-content;
    margin: 20px auto 0 auto;
    font-weight: 500;

    span {
      font-size: 22px;
      color: #ff7300;
    }
  }
}
</style>