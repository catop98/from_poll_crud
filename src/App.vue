<template>
  
  <HelloWorld msg="Welcome to Your Vue.js App"/>
  <SurveyForm></SurveyForm>
  <CopySurvey></CopySurvey>
  <Two_Copy_Form></Two_Copy_Form>
  <ImageCustom></ImageCustom>
  
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue';

//import SurveyForm from './components/SurveyForm.vue';

import CopySurvey from './components/CopySurvey.vue';

//import ImageCustom from './components/ImageCustom.vue';



/* eslint-disable */
export default {
  name: 'App',
  components: {
    //HelloWorld,
    //SurveyForm,
    CopySurvey,
    //ImageCustom
    
    /* eslint-disable */
    
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
