<template>
  <div class="starting-exercise">
    <div class="starting-exercise__container">


      <div
        class="starting-exercise__image-container"
        v-if="isExistingExercise"
      >
        <div
          v-for="(exercise, index) in currentExercises"
          :key="exercise.id"
        >
          <img
            class="starting-exercise__image"
            :class="{ 'starting-exercise__image--active': index === currentIndex }"
            :src="exercise.imagePreview"
            alt=""
          >
          <p
            v-if="index === currentIndex"
            class="starting-exercise__time"
            :class="{ 'starting-exercise__time--active': currentTime }"
          >Выполнение: <span>{{ currentTime }} сек.</span></p>
          <p
            v-else
            class="starting-exercise__time"
          >Выполнение: {{ exercise.time }} сек.</p>

        </div>
      </div>


      <div
        v-else
        class="starting-exercise__image-preview"
      >
        <span class="pi pi-camera"></span>
      </div>

      <button
        class="starting-exercise__button-next"
        @click="skipSlide()"
      >Следующее упражнение</button>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  props: ["currentExercises", "infoComplex", "isExistingExercise"],

  data() {
    return {
      currentTime: 0,
      timer: null as number | null,
      currentIndex: 0
    }
  },

  computed: {
    currentExercise() {
      return this.currentExercises[this.currentIndex]
    },
  },

  methods: {
    startTimer(time: number) {
      this.currentTime = time;
      this.timer = setInterval(() => {
        this.currentTime--;

        if (this.currentTime <= 0) {

          this.stopTimer();
          this.nextSlide()
        }
      }, 1000);
    },

    nextSlide() {
      if (this.currentIndex < this.currentExercises.length - 1) {
        this.currentIndex++;
        this.startTimer(Number(this.currentExercise.time));
      } else {
        this.$emit('nextSlide')
      }
    },

    stopTimer() {
      if (this.timer) {
        clearInterval(this.timer);
        this.timer = null;
      }
    },

    skipSlide() {
      this.stopTimer();
      this.currentTime = 0
      this.$emit('nextSlide')
    }
  },
  watch: {
    isExistingExercise: {
      handler(newVal) {
        if (newVal) {
          this.currentIndex = 0;
          this.startTimer(Number(this.currentExercise.time));
        }

      }
    }
  }
}
</script>

<style lang="scss">
.starting-exercise {

  &__container {
    background-color: #f7f7f7;
    padding: 30px 24px;
    box-shadow: 0px 0px 8px 0px rgba(0, 0, 0, 0.2);
    border-radius: 6px;
  }

  &__image-container {
    display: flex;
    justify-content: center;
    column-gap: 30px;
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
    object-fit: cover;
    transition: all 0.2s;

    span {
      font-size: 46px;
      color: #5e5e5e;
    }
  }

  &__time {
    font-size: 17px;
    font-weight: 500;
    width: fit-content;
    margin: 14px auto 0 auto;
  }

  &__time--active {
    font-size: 18px;

    span {
      font-size: 22px;
      color: #ff7300;
    }
  }

  &__image--active {
    transform: scale(1.1);
  }

  &__button-next {
    margin-top: 30px;
    display: block;
    margin-left: auto;
  }
}
</style>