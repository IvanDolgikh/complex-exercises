<template>

  <div class="main">

    <div class="main__form-container">
      <ActivityAdding
        class="activity-adding"
        @save-data="saveData"
      />
      <ActivityForm
        class="activity-form"
        :arrayExercises="arrayExercises"
        @send-info-complex="sendInfoComplex"
      />
    </div>

    <ActivityLearning
      class="activity-learning"
      :data="{
          arrayExercises,
          infoComplex,
          isFilled,
          currentStage
        }"
      @transfer-current-exercise="transferCurrentExercise"
    />

    <ActivityStartingExercise
      :currentExercises="currentExercises"
      :infoComplex="infoComplex"
      :isExistingExercise="isExistingExercise"
      @next-slide="nextSlide"
    />

  </div>

  <RouterView />
</template>

<script lang="ts">
import ActivityForm from './components/ActivityForm.vue'
import ActivityLearning from './components/ActivityLearning.vue'
import ActivityAdding from './components/ActivityAdding.vue'
import ActivityStartingExercise from './components/ActivityStartingExercise.vue'
import { defineComponent } from 'vue'

interface DataItem {
  id: number
  imagePreview: string
  time: string
  type: string
}

export default defineComponent({
  components: {
    ActivityForm,
    ActivityLearning,
    ActivityAdding,
    ActivityStartingExercise
  },

  data() {
    return {
      arrayExercises: [
        {
          id: 1,
          imagePreview: 'src/assets/images/6c7bb734-9050-432a-9597-d40147e275f5.jpg',
          time: '44',
          type: '1',
        },
        {
          id: 1,
          imagePreview: 'src/assets/images/62e255cd10fe2_lg.jpg',
          time: '60',
          type: '1',
        },
        {
          id: 1,
          imagePreview: 'src/assets/images/914babf7-2b1c-4d34-ae36-f056725849f6.jpeg',
          time: '24',
          type: '1',
        },
        {
          id: 1,
          imagePreview: 'src/assets/images/cc9efe4f-eb8d-4fb4-b4de-f17a0bd6f164.jpeg',
          time: '5',
          type: '2',
        },
        {
          id: 2,
          imagePreview: 'src/assets/images/e5ff66445d970e9103f766ce95761403.jpg',
          time: '7',
          type: '2',
        },
        {
          id: 3,
          imagePreview: 'src/assets/images/front-view-sportsman-with-copy-space_23-2148222189.avif',
          time: '8',
          type: '2',
        },
        {
          id: 3,
          imagePreview: 'src/assets/images/hippopotamus.jpeg',
          time: '5',
          type: '3',
        },
        {
          id: 3,
          imagePreview: 'src/assets/images/side-view-sportsman-doing-exercises-windowsill_171337-19706.avif',
          time: '2',
          type: '3',
        }
      ] as DataItem[],

      infoComplex: {
        firstTypeAmount: 3,
        secondTypeAmount: 2,
        thirdTypeAmount: 2,
        timeForExercise: 3 as number,
      },

      isFilled: false as boolean,
      isExistingExercise: false as boolean,
      currentExercises: [] as DataItem[],
      currentStage: 1
    }
  },



  methods: {
    saveData(data: DataItem) {
      if (data) {
        this.arrayExercises.push(data)
      }
    },

    sendInfoComplex(infoComplex: any) {
      this.infoComplex.firstTypeAmount = infoComplex.firstType
      this.infoComplex.secondTypeAmount = infoComplex.secondType
      this.infoComplex.thirdTypeAmount = infoComplex.thirdType
      this.infoComplex.timeForExercise = infoComplex.time
      this.isFilled = true
    },

    transferCurrentExercise(exercises: DataItem[]) {
      this.currentExercises = exercises
      this.isFilled = false
      this.isExistingExercise = true
    },

    nextSlide() {
      if (this.currentStage < 3) {
        this.currentStage++
        this.isExistingExercise = false
        this.isFilled = true
      }

    }
  }
})
</script>



<style
  scoped
  lang="scss"
>
.main {
  &__form-container {
    display: flex;
    justify-content: center;
    column-gap: 50px;
  }
}

.activity-form,
.activity-adding,
.activity-learning {
  margin-bottom: 30px;
  width: 100%
}


header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
