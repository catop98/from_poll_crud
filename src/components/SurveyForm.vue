<template>
  
    <form @submit.prevent="addSurveyData" class="container">
    
      <div class="wrapper-stepper">
          <div class="stepper">
              <div class="stepper-progress">
                  <div class="stepper-progress-bar" :style="'width:' + stepperProgress "></div>
              </div>

              <div class="stepper-item col-md-1" :class="{ 'current': currentStep == item, 'success': currentStep > item }" v-for="item in 6" :key="item">
                  <div class="stepper-item-counter">
                      <img class="icon-success" src="https://www.seekpng.com/png/full/1-10353_check-mark-green-png-green-check-mark-svg.png" alt="">
                      <span class="number">
                          {{ item }}
                      </span>
                  </div>
                  <span class="stepper-item-title row ">
                      Paso {{ item }}
                  </span>
              </div>
          </div>

          <div class="stepper-content " v-for="item in 6" :key="item">
              <div class="stepper-pane" v-if="currentStep == item">
              <h2><p class="font-weigth-light">{{ formSteps[currentStep].title }}</p></h2>

             <!-- STEP ONE -->
              <div class="step-form-one" v-if="currentStep === 1">
                    <div v-for="n in numbers" :key="n.id">

                          <input class="form-step-one" type="radio" v-model="picked" :value="n.id" >
                        {{n.number}}
                    </div>
              </div>

                <!-- STEP TWO -->
              <div v-if="currentStep === 2">
                <div class="input-container"
                v-for="(field, index) in formSteps[currentStep].fields"
                :key="'field'+index">

                <input class="input-text" type="text" :class="{'wrong-input': !field.valid}" v-model="field.value" placeholder="Escribe tu respuesta aca.." required>
              </div>
              </div>

              <!-- STEP THREE -->
              <div v-if="currentStep === 3">
                <div
                  v-for="star in maxstars"
                  :key="star"
                  @mouseover="hoverStar(star)"
                  @mouseleave="mouseLeftStar"
                  :class="[{ 'active': star <= stars }]"
                  @click="rate(star)" class="star"
                  >
                  <font-awesome-icon :class="`fa-${starsize}`" :icon="[star <= stars ? 'fas' : 'fas', 'star']"/>
      
                  </div>
                <span v-if="hasdescription && star_desc" :class="star_desc.class">{{star_desc.text}}</span>
                <span v-else-if="!star_desc" class="nostar_desc">No Description</span>
              </div>


              <!-- STEP FOUR -->
              <div class="step-form-four" v-if="currentStep === 4">
                <table>
                  <tbody>
                    <tr class="options-form" v-for="d in options" :key="d.id">
                      <td >
                       <label class="checkbox">
                        <input type="checkbox" v-model="selected" :value="d.id" >
                        <span class="check"></span>
                       </label>
                        
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

                <input class="input-text" type="text" :class="{'wrong-input': !field.valid}" v-model="field.value" placeholder="Escribe tu respuesta aca.." required>
              </div>
              </div>
            
              
              </div>
          </div>

          <div class="controls ">
              <button  type="button" class="btn font-weigth-light "  @click="previuosStep" :disabled="currentStep == 1">
                  Anterior
              </button>
              <button class="btn btn--green-1 font-weigth-light" @click="nextStep" :disabled="currentStep == 6">
                  Siguiente
              </button>
              <button class="btn btn--green-1" v-if="currentStep === 6">Enviar</button>
          </div>
      </div>








      
    </form>
  
   
</template>


<script>
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
    surveyData: [],
    selected: [],
    picked: [],
    rated: [],
    stars: [
      {
        id: '1',

      },
      {
        id: '2',

      },
      {
        id: '3',

      },
      {
        id: '4',

      },
      {
        id: '5',

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
        letter: 'A.',
        option: 'Complicado proceso de registro.'
      },
      {
        id: '2',
        letter: 'B.',
        option: 'Problemas de audio'
      },
      {
        id: '3',
        letter: 'C.',
        option: 'Retraso en la carga de videos de sesión.'
      },
      {
        id: '4',
        letter: 'D.',
        option: "No se pudo acceder a la aplicación móvil."
      },
      {
        id: '5',
        letter: 'E.',
        option: "No podía usar la función de encuestas."
        },
      {
        id: '6',
        letter: 'F.',
        option: "No experimentó ningún problema."
      },
      {
        id: '7',
        letter: 'G.',
        option: "No se pudo usar la función de preguntas y respuestas/chat"
      },
    ],
   
    formSteps: [
          {id: '1',},
          {
            id: '2',
            title: "¿Qué tan probable es que recomiende eventos de eventMB a un amigo o colega?",
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
            id: '3',
            title: "¿ Porqué elegiste 7?",
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
            title: "Califica la plataforma de eventos",
            fields: [
              {
                label: "1: bajo ---- 5: excelente",
                value: '',
                valid: true,
                pattern: /.+/
              }
            ]
          },
          {
            id: '5',
            title: "¿ Tuviste alguna dificultad tecnica?",
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
            id: '6',
            title: "¿Hay algo más que le gustaría compartir con nosotros?",
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

  methods: {
    rate(star) {

      if (this.disabled) {
        return;
      }
      if (star <= this.maxstars && star >= 0) {
        this.stars = this.stars === star ? star - 1 : star;
        this.surveyData.push(star)
        console.log(star)
      }
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
    nextStep() {
      try {
        const survey = {
          title: this.formSteps[this.currentStep].title,
          fields: this.formSteps[this.currentStep].fields[0].value
        }
        this.surveyData.push(survey)
        this.currentStep++
        console.log('Next step successfully')
      } catch (error) {
          console.log(error.response.data);
          console.log(error.response.status);
          console.log(error.response.headers);
      }
    },
  previuosStep() {
    this.currentStep--
  },
    addSurveyData() {
      const survey = {
        title: this.formSteps[this.currentStep].title,
        fields: this.formSteps[this.currentStep].fields[0].value
      }
      console.log(this.surveyData)
      console.log('Question 4, select: ', this.selected)
      console.log('Question 1, pick: ' ,this.picked)
      console.log('Rating: ' ,this.rating)
      this.surveyData.push(survey)
      console.log('Rated: ' ,this.rated)
    },
  }
}

</script>

<style>
.stepper-pane{
  font-family:'Times New Roman', Times, serif;
  
}

.selector-for-some-widget {
  box-sizing: content-box;
}



body {
  background: linear-gradient(90deg,#871a89,#e11d7a);
  color: #ffffff;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family:monospace;
}

.tx-green-1 {
  color: #871a89;
  font-weight: 600;
}

.wrapper-stepper {
  background-color: #fff;
  padding: 60px;
  border-radius: 32px;
  box-shadow: rgba(0, 0, 0, 0.09);
}

.stepper {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 660px;
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
  background-color: #871a89;
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
  border-color: #871a89;
  background-color: #e11d7a;
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
  color: #871a89;
}

.stepper-item.current .stepper-item-counter {
  border-color: #871a89;
  background-color: #871a89;
  color: #fff;
  font-weight: 600;
}
.stepper-item.current .stepper-item-title {
  color: #818181;
}

.stepper-pane {
  color: #333;
  text-align: center;
  padding: 120px 60px;
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
  font-size: 1.3rem;
  color: #fff;
  background-color: #871a89;
  border-color: #871a89;
  
}
.btn:disabled {
  opacity: 0.5;
  pointer-events: none;
}
.btn--green-1 {
  background-color: #871a89;
  border-color: #871a89;
  color: #fff;
  margin-left: auto;
  size: 30px;
}
.input-text{
  outline: none;
  border:none;
  border-bottom:1px solid rgb(14, 6, 6);
  
}




.step-form-one{
  display:flex;
  justify-content:center ;
  font-size: 16px;
  color: #0c0505;
  
  

}
.form-step-one {
margin-bottom: 20px;
padding: 20px 0;
margin-bottom: 20px;
margin-left: 20px;
cursor: pointer;
content: "";  
width: 20px;
height: 20px;

}

.stepper-item-counter{
  display: flex;
  justify-content: center;
}
.font-weigth-light{
  font-size: 18px;
  font-weight: bold;
}
.step-form-four{
  display: flex;
  justify-content: center;
}
.checkbox {
    display: block;
    position: relative;
    padding-left: 35px;
    margin-bottom: 12px;
    cursor: pointer;
    font-size: 22px;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}

/* hide the browser's default checkbox */
.checkbox input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
}

/* create custom checkbox */
.check {
    position: absolute;
    top: 0;
    left: 0;
    height: 25px;
    width: 25px;
    background-color: #eee;
    border: 1px solid rgb(12, 9, 9);
}

/* on mouse-over, add border color */
.checkbox:hover input ~ .check {
    border: 2px solid #e11d7a;
}

/* add background color when the checkbox is checked */
.checkbox input:checked ~ .check {
    background-color: #e11d7a;
    border:none;
}

/* create the checkmark and hide when not checked */
.check:after {
    content: "";
    position: absolute;
    display: none;
}

/* show the checkmark when checked */
.checkbox input:checked ~ .check:after {
    display: block;
}

/* checkmark style */
.checkbox .check:after {
    left: 9px;
    top: 5px;
    width: 5px;
    height: 10px;
    border: solid white;
    border-width: 0 3px 3px 0;
    -webkit-transform: rotate(45deg);
    -ms-transform: rotate(45deg);
    transform: rotate(45deg);
}
.options-form{
  text-align: left;
}
.stepper-pane{
  background:linear-gradient(90deg,#fdc6ff,#ce9db4);
  border-radius: 15px;
  
}



/* eslint-disable */
</style>
