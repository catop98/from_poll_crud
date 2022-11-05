
<template>
  <div class="body-form">
    <form @submit.prevent="">
    <div class="card">
      <div class="card-body">
        <div class="card-title">
          <div class="stepper">
              <div class="stepper-progress">
                  <div class="stepper-progress-bar" :style="'width:' + stepperProgress "></div>
              </div>
              <div class="stepper-item" :class="{ 'current': currentStep == item, 'success': currentStep > item }" v-for="item in 6" :key="item">
                  <div class="stepper-item-counter">
                      <img class="icon-success" src="https://www.seekpng.com/png/full/1-10353_check-mark-green-png-green-check-mark-svg.png" alt="">
                      <span class="number">
                          {{ item }}
                      </span>
                  </div>
                  <span class="stepper-item-title">
                      Paso {{ item }}
                  </span>
              </div>
          </div>
            <div class="card-body2">
              <div class="stepper-content" v-for="item in 6" :key="item">
                <div class="stepper-pane" v-if="currentStep == item">
                  <h2>{{ formSteps[currentStep].title }}</h2> 

                  <!-- STEP ONE -->
                  <div v-if="currentStep === 1">
                        <div v-for="n in numbers" :key="n.id">
                          <input type="radio" v-model="picked" :value="n.id" >
                          <label>{{n.number}}</label>

                        </div>
                  </div>

                  <!-- STEP TWO -->
                  <div v-if="currentStep === 2">
                    <div class="input-container"
                    v-for="(field, index) in formSteps[currentStep].fields"
                    :key="'field'+index">

                    <input type="text" :class="{'wrong-input': !field.valid}" v-model="field.value" placeholder="Escribe tu respuesta aca.." required>
                  </div>
                  </div>

                  <!-- STEP THREE -->
                  <div v-if="currentStep === 3">
                    <div class="div-rate">
                      <div
                        v-for="star in maxstars"
                        :key="star"
                        @mouseover="hoverStar(star)"
                        @mouseleave="mouseLeftStar"
                        :class="[{ 'active': star <= stars }]"
                        @click="rate(star)"
                        class="star"
                        >
                        <font-awesome-icon :class="`fa-${starsize}`" :icon="[star <= stars ? 'fas' : 'fas', 'star']"/>
                      </div>
                      <span v-if="hasdescription && star_desc" :class="star_desc.class">{{star_desc.text}}</span>
                      <span v-else-if="!star_desc" class="nostar_desc">No Description</span>
                    </div>
                    </div>

                  <!-- STEP FOUR -->
                  <div v-if="currentStep === 4">
                    <table>
                      <tbody>
                        <tr
                          v-for="d in options"
                          :key="d.id"
                          @click="getSelect(d.option)"
                          >
                          <td>
                            <input type="checkbox" v-model="selected" :value="d.id" >
                          </td>
                          <td>{{d.letter}}</td>
                          <td>{{d.option}}</td>
                        </tr>
                      </tbody>
                    </table>
                  </div>

                  <!-- STEP FIVE -->
                  <div v-if="currentStep === 5">
                    <div class="input-container"
                      v-for="(field, index) in formSteps[currentStep].fields"
                      :key="'field'+index">

                      <input type="text" :class="{'wrong-input': !field.valid}" v-model="field.value" placeholder="Escribe tu respuesta aca.." required>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div v-if="currentStep <= 5" class="controls">
              <button
                class="btn"
                @click="previuosStep"
                :disabled="currentStep == 1"
                >
                  Anterior
              </button>
              <button
                class="btn btn--green-1"
                @click="nextStep"
                :disabled="currentStep == 5"
                >
                  Siguiente
              </button>
              <button
                class="btn btn--green-1"
                @click="addSurveyData"
                v-if="currentStep === 5"
                >
                Enviar
                </button>
          </div>
    </div>
    </form>

    <!-- <div class="card-body px-0 pt-0 pb-2">
      <div class="table-responsive p-0">
        <table class="table align-items-center mb-0">
          <thead>
            <tr>
              <th class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">
                Pregunta
              </th>
              <th class="text-uppercase text-secondary text-xxs font-weight-bolder opacity-7">
                Respuesta
              </th>
            </tr>
          </thead>
          <tbody v-if="currentStep > 4">
            <tr
              v-for="survey in surveyData"
              :key="survey.id"
              >
              <td>
                <div class="d-flex px-2 py-1">
                  <div class="d-flex flex-column justify-content-center">
                    <p class="text-xs text-secondary mb-0">{{survey.title}}</p>
                  </div>
                </div>
              </td>
              <td>
                <div class="d-flex px-2 py-1">
                  <div class="d-flex flex-column justify-content-center">
                    <p class="text-xs text-secondary mb-0">{{survey.fields}}</p>
                  </div>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div> -->

  </div>
</template>

<script>
import axios from 'axios'


export default {
  props: {
    star: {
      type: Number,
    },
    maxstars: {
      type: Number,
      default: 5
    },
    hasresults: {
      type: Boolean,
      default: true
    },
    hasdescription: {
      type: Boolean,
      default: true
    },
    ratingdescription: {
      type: Array,
      default: () => {
        return [
          {
            text: "Poor",
            class: "star-poor"
          },
          {
            text: "Below Average",
            class: "star-belowAverage"
          },
          {
            text: "Average",
            class: "star-average"
          },
          {
            text: "Good",
            class: "star-good"
          },
          {
            text: "Excellent",
            class: "star-excellent"
          }
        ];
      }
    },
    starsize: {
      type: String,
      default: "2x"
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  data: () => ({
    currentStep: 1,
    totalSurveyData: [],
    inputData: [],
    selected: [],
    picked: [],
    rated: [],
    star_desc: false,
    disable: false,
    stars: [
      {
        id: '1',
        star: '1'
      },
      {
        id: '2',
        star: '2'
      },
      {
        id: '3',
        star: '3'
      },
      {
        id: '4',
        star: '4'
      },
      {
        id: '5',
        star: '5'
      }
    ],
    numbers: [
      {
        id: '1',
        number: '1'
      },
      {
        id: '2',
        number: '2'
      },
      {
        id: '3',
        number: '3'
      },
      {
        id: '4',
        number: '4'
      },
      {
        id: '5',
        number: '5'
      },
      {
        id: '6',
        number: '6'
      },
      {
        id: '7',
        number: '7'
      },
      {
        id: '8',
        number: '8'
      },
      {
        id: '9',
        number: '9'
      },
      {
        id: '10',
        number: '10'
      },
    ],
    options: [
      {
        id: '1',
        letter: 'A',
        option: 'Complicated registration process'
      },
      {
        id: '2',
        letter: 'B',
        option: 'Audio issues'
      },
      {
        id: '3',
        letter: 'C',
        option: 'Delay in loading session videos'
      },
      {
        id: '4',
        letter: 'D',
        option: "Couldn't access the mobile app"
      },
      {
        id: '5',
        letter: 'E',
        option: "Couldn't use polls feature"
      },
      {
        id: '6',
        letter: 'F',
        option: "did not experience any issues"
      },
      {
        id: '7',
        letter: 'G',
        option: "Couldn't use Q&A/Chat feature"
      },
    ],
    formSteps: [
          {id: '1',},
          {
            id: '2',
            title: "How likely are you to recommend event to a friend or colleague?",
          },
          {
            id: '3',
            title: "Why did you choose 7?",
            fields: [
              {
                label: "write your answer here",
                value: '',
                valid: true,
                pattern: /.+/
              }
            ]
          },
          {
            id: '4',
            title: "Please rate the event platform",
          },
          {
            id: '5',
            title: "Did you experience any technical difficulties?",
          },
          {
            id: '6',
            title: "Is there anything else you would like to share with us?",
            fields: [
              {
                label: "write your answer here",
                value: '',
                valid: true,
                pattern: /.+/
              }
            ]
          },
          {
            id: '7',
            title: "Thank you for participating!",
          }
        ],
  }),
  computed: {
    stepperProgress() {
      return ( 100 / 3 ) * ( this.step - 1 ) + '%'
    }
  },
  mounted() {
    // this.getSurvey()
  },
  created() {
    // console.log(this.star)
  },
  methods: {
    rate(star) {
      if (this.disabled) {
        return;
      }
      if (star <= this.maxstars && star >= 0) {
        this.stars = this.stars === star ? star - 1 : star;
      }
      const catchRate = {
        title: this.formSteps[this.currentStep].title,
        rate: star
      }
      this.rated.push(catchRate)
    },
    hoverStar(star) {
      if (this.disabled) {
        return;
      }
      if (star <= this.maxstars && star >= 0) {
        this.star_desc = this.ratingdescription[star - 1];
      }
    },
    mouseLeftStar() {
      if (this.disabled) {
        return;
      }
      if (this.stars) {
        this.star_desc = this.ratingdescription[this.stars - 1];
        return this.star_desc;
      } else {
        this.star_desc = "";
      }
    },
    getRatingDesc(star) {
      if (star) {
        this.star_desc = this.ratingdescription[star - 1];
      }
      return this.star_desc;
    },
    getSelect(d) {
      this.options.option = d
        const catchSelect = {
          title: this.formSteps[this.currentStep].title,
          option: d
        }
        this.selected.push(catchSelect)
        console.log(this.selected)
    },
    nextStep() {
        if(this.currentStep === 2) {
          const survey = {
            title: this.formSteps[this.currentStep].title,
            fields: this.formSteps[this.currentStep].fields[0].value
          }
          this.inputData.push(survey)
        }
        if(this.currentStep === 5) {
          const survey = {
            title: this.formSteps[this.currentStep].title,
            fields: this.formSteps[this.currentStep].fields[0].value
          }
          this.inputData.push(survey)
        }

        this.currentStep++
    },
    previuosStep() {
      this.currentStep--
    },
    async addSurveyData() {
        const surveyData = {
          pick: this.picked,
          rate: this.rated,
          select: this.selected,
          input: this.inputData
        }
        this.totalSurveyData.push(surveyData)
        console.log('totalSurveyData: ',this.totalSurveyData)
        this.currentStep++

        // await axios.post('http://localhost:5000/api/surveys', {survey}, getToken)
        //   .then(response => {
        //     console.log(response.data)
        //     response.data
        //   })

        // this.currentStep++
      },
      // async getSurvey() {
      //   try {
      //     const getToken = {
      //       headers: {
      //         Authorization: process.env.WP_API_KEY,
      //         'Content-Type': 'application/json; charset=utf-8'
      //       }
      //     }
      //     this.surveyData = await axios.get('http://localhost:5000/api/surveys', getToken)
      //       then(response => {
      //         response.data.surveys
      //       })
      //   } catch (error) {
      //     console.log(error.response.data);
      //     console.log(error.response.status);
      //     console.log(error.response.headers);
      //   }
      // }
  }
}

</script>

<style>
.card {
  box-shadow: none;
  width: 100vh;
  height: 80vh;
}
@media (max-width: 675px) {
  .card {
    width: auto;
  }
  .stepper-item {
    width: 50px;
  }
}

.body-form {
  background-image: linear-gradient(60deg, #abecd6 0%, #fbed96 100%);
  color: #ffffff;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: sans-serif;
}
.controls {
  padding: 25px;
}

.tx-green-1 {
  color: #75cc65;
  font-weight: 600;
}

/* .wrapper-stepper {
  background-color: #fff;
  padding: 60px;
  border-radius: 32px;
  box-shadow: rgba(0, 0, 0, 0.09);
} */

.stepper {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  position: relative;
  z-index: 0;
  margin-bottom: 24px;

}
.stepper-progress {
  position: absolute;
  background-color: #c5c5c5;
  height: 2px;
  z-index: -1;
  left: 0;
  right: 0;
  margin: 0 auto;
}
.stepper-progress-bar {
  position: absolute;
  left: 0;
  height: 100%;
  width: 0%;
  background-color: #75cc65;
  transition: all 500ms ease;
}

.stepper-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #c5c5c5;
  transition: all 500ms ease;
}
.stepper-item-counter {
  height: 68px;
  width: 68px;
  display: grid;
  place-items: center;
  background-color: #fff;
  border-radius: 100%;
  border: 2px solid #c5c5c5;
  position: relative;
}
.stepper-item-counter .icon-success {
  position: absolute;
  opacity: 0;
  transform: scale(0);
  width: 24px;
  transition: all 500ms ease;
}
.stepper-item-counter .number {
  font-size: 22px;
  transition: all 500ms ease;
}
.stepper-item-title {
  position: absolute;
  font-size: 14px;
  bottom: -24px;
}

.stepper-item.success .stepper-item-counter {
  border-color: #75cc65;
  background-color: #c8ebc1;
  color: #fff;
  font-weight: 600;
}
.stepper-item.success .stepper-item-counter .icon-success {
  opacity: 1;
  transform: scale(1);
}
.stepper-item.success .stepper-item-counter .number {
  opacity: 0;
  transform: scale(0);
}
.stepper-item.success .stepper-item-title {
  color: #75cc65;
}

.stepper-item.current .stepper-item-counter {
  border-color: #75cc65;
  background-color: #75cc65;
  color: #fff;
  font-weight: 600;
}
.stepper-item.current .stepper-item-title {
  color: #818181;
}

.stepper-pane {
  color: #333;
  text-align: center;
  box-shadow: 0 8px 12px rgba(0, 0, 0, 0.09);
  margin: 40px 0;
}

.controls {
  display: flex;
}

.btn {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 6px 16px;
  border: 1px solid;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  line-height: 1.5;
  transition: all 150ms;
  border-radius: 4px;
  width: fit-content;
  font-size: 0.75rem;
  color: #333;
  background-color: #f0f0f0;
  border-color: #f0f0f0;
}
.btn:disabled {
  opacity: 0.5;
  pointer-events: none;
}
.btn--green-1 {
  background-color: #75cc65;
  border-color: #75cc65;
  color: #fff;
  margin-left: auto;
}

.rating {
	 font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
 ul.list li, span {
	 display: inline-block;
	 margin: 2px;
}
 .list {
	 margin: 0 0 5px 0;
	 padding: 0;
	 list-style-type: none;
}
 .list:hover .star {
	 color: #f3d23e;
}
 .list span {
	 width: 130px;
	 margin-left: 5px;
	 padding: 5px;
	 color: #fff;
	 border-radius: 2px;
	 font-size: 13px;
	 text-align: center;
	 font-weight: bold;
	 transition: 0.2s;
	 line-height: 25px;
}
 .list.disabled:hover .star {
	 color: black;
	 cursor: default;
}
 .list.disabled:hover .star.active {
	 color: #f3d23e;
}
 .star {
	 cursor: pointer;
}
 .star:hover ~ .star:not(.active) {
	 color: inherit;
}
 .active {
	 color: #f3d23e;
}
 .star-poor {
	 background: #bababa;
}
 .star-belowAverage {
	 background: #f5c357;
}
 .star-average {
	 background: #f3d23e;
}
 .star-good {
	 background: #c1d759;
}
 .star-excellent {
	 background: #48964d;
}
 .nostar_desc {
	 background: #000;
}

</style>
