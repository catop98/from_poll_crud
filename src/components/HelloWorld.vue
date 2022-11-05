
<template>
  <div class="container">
    <form @submit.prevent="addSurveyData">
      <div class="wrapper-stepper">
          <div class="stepper">
              <div class="stepper-progress">
                  <div class="stepper-progress-bar" :style="'width:' + stepperProgress "></div>
              </div>

              <div class="stepper-item" :class="{ 'current': currentStep == item, 'success': currentStep > item }" v-for="item in 5" :key="item">
                  <div class="stepper-item-counter">
                      <img class="icon-success" src="https://www.seekpng.com/png/full/1-10353_check-mark-green-png-green-check-mark-svg.png" alt="">
                      <span class="number">
                          {{ item }}
                      </span>
                  </div>
                  <span class="stepper-item-title col-md-1">
                      Paso {{ item }}
                  </span>
              </div>
          </div>

          <div class="stepper-content" v-for="item in 5" :key="item">
              <div class="stepper-pane" v-if="currentStep == item">
              <h2>{{ formSteps[currentStep].title }}</h2>
              <div class="input-container"
                v-for="(field, index) in formSteps[currentStep].fields"
                :key="'field'+index">
                <label class="input-label">{{ field.label }}</label>
                <input type="text" :class="{'wrong-input': !field.valid}" v-model="field.value" required>
              </div>
              </div>
          </div>

          <div class="controls">
              <button class="btn col-md-1" @click="previuosStep" :disabled="currentStep == 1">
                  Anterior
              </button>
              <button class="btn btn--green-1 col-md-2" @click="nextStep" :disabled="currentStep == 4">
                  Siguiente
              </button>
              <button class="btn btn--green-1 col-md-3" v-if="currentStep === 4">Enviar</button>
          </div>
      </div>
    </form>

    <div class="card-body px-0 pt-0 pb-2">
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
    </div>

  </div>
</template>

<script>
export default {
  data: () => ({
    currentStep: 1,
    surveyData: [],
    formSteps: [
          {
            title: "How likely are you to recommend event to a friend or colleague?",
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
            title: "Please rate the event platform",
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
            title: "did you experience any technical difficulties",
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
            title: "Did you experience any technical difficulties?",
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
    nextStep() {
      const survey = {
        title: this.formSteps[this.currentStep].title,
        fields: this.formSteps[this.currentStep].fields[0].value
      }
      this.surveyData.push(survey)
    this.currentStep++
    // this.formSteps.save()
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
      this.surveyData.push(survey)
      this.currentStep++
    },
  }
}

</script>

<style>

body {
  background: linear-gradient(90deg,#871a89,#e11d7a);
  color: #ffffff;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: sans-serif;
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
  font-size: 0.75rem;
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
}
</style>
