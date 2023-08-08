<script setup>
import {reactive, ref} from "vue";
import * as EmailValidator from 'email-validator';
import AppDashboard from "@/components/AppDashboard.vue";
 const registrationFlag = ref(false)
 const dashboardFlag = ref(false)
 const data = reactive({
   'email':'',
   'password':'',
   'confirmPassword':'',
 })

const validationFlag = reactive({
  'emailValidationFlag':false,
  'passwordValidationFlag':false,
  'confirmPasswordValidationFlag':false,
})

const emailValidationMessage = ref('');
const passwordValidationMessage = ref('');
const confirmValidationMessage = ref('');

function checkConfirmPass(){
   if(data.password===data.confirmPassword && data.password !== "" && data.confirmPassword !=='' ){
     return true
   }
   else{
     return false
   }
}
// check validation
function emailValidation(){
  const checkEmail = EmailValidator.validate(data.email);
    if(checkEmail){
      emailValidationMessage.value="Thank you for entering correct email"
      validationFlag.emailValidationFlag=true;
    }
    else {
      emailValidationMessage.value="Please enter correct email"
      validationFlag.emailValidationFlag=false;
    }

}

function ValidationPassword(){
  if(data.password === ''){
    passwordValidationMessage.value='Please Enter Password'
    validationFlag.passwordValidationFlag = false
  }
  else if(data.password !== '') {
    passwordValidationMessage.value='Thank you for enter password'
    validationFlag.passwordValidationFlag = true
  }

}

function validationConfirmPassword(){
  if(data.confirmPassword === ''){
    confirmValidationMessage.value='Please Enter Confirm Password'
    validationFlag.confirmPasswordValidationFlag = false
  }
  else if(checkConfirmPass()===false){
    confirmValidationMessage.value='Password and Confirm Password are not same'
    validationFlag.confirmPasswordValidationFlag = false
  }
  else {
    confirmValidationMessage.value='Thank you for confirming your password'
    validationFlag.confirmPasswordValidationFlag = true
  }
}

function showDashboard(){
  dashboardFlag.value=true
}



</script>

<template>
  <template v-if="dashboardFlag===true">
    <AppDashboard/>
  </template>
  <section v-if="dashboardFlag===false">
    <div class="container py-5 h-100">
      <div class="row d-flex justify-content-center align-items-center h-100">
        <div class="col-12 col-md-8 col-lg-6 col-xl-5">
          <div class="card bg-dark text-white" style="border-radius: 1rem;">
            <div class="card-body p-5 ">
              <div class="mb-md-5 mt-md-4 pb-1">
                <h2 class="fw-bold mb-2 text-uppercase text-center">{{registrationFlag===false ? 'login':'registration'}}</h2>
                <p class="text-white-50 mb-4 text-center">Please enter correct data then login or signup button enable</p>

                <div class=" form-white mb-3">
                  <label for="typeEmailX">Email</label>
                  <input @input="emailValidation()" type="email" id="typeEmailX" class="form-control form-control-lg" v-model="data.email" />
                  <span :class="(validationFlag.emailValidationFlag===true)?'text-success':'text-danger'">{{emailValidationMessage}}</span>
                </div>
                <div class="form-white mb-3">
                  <label  for="typePasswordX">Password</label>
                  <input @input="ValidationPassword()" :style="checkConfirmPass()===true? 'color:green; outline-style: solid; outline-color:green':''" type="password" id="typePasswordX" class="form-control form-control-lg" v-model="data.password" />
                  <span :class="validationFlag.passwordValidationFlag===true?'text-success':'text-danger'">{{passwordValidationMessage}}</span>
                </div>
                <div class="form-white mb-3" v-show="registrationFlag===true">
                  <label  for="confirmPasswordX">Confirm Password</label>
                  <input @input="validationConfirmPassword()" type="password" id="confirmPassword" class="form-control form-control-lg" :style="checkConfirmPass()===true? 'color:green':'color:red'" v-model="data.confirmPassword" />
                  <span :class="validationFlag.confirmPasswordValidationFlag===true?'text-success':'text-danger'">{{confirmValidationMessage}}</span>
                </div>
                <div class="button text-center">

                  <p class="small mb-2 pb-lg-2"><a class="text-white-50" href="#!">Forgot password?</a></p>

                  <button @click="showDashboard()" v-show="registrationFlag===false" class="btn btn-outline-light btn-lg px-5 text-center" :class="(validationFlag.emailValidationFlag===false || validationFlag.passwordValidationFlag===false)? 'disabled':''" type="submit"> Login</button>
                  <button @click="showDashboard()" v-show="registrationFlag===true"  class="btn btn-outline-light btn-lg px-5 text-center" :class="(validationFlag.emailValidationFlag===false || validationFlag.passwordValidationFlag===false || validationFlag.confirmPasswordValidationFlag===false)? 'disabled':'' " type="submit"> Sign Up</button>
                </div>
              </div>
              <div class="text-center">
                <p @click="registrationFlag=true" v-show="registrationFlag==false" class="mb-0">Don't have an account? <a href="#!" class="text-white-50 fw-bold">Sign Up</a>
                </p>
                <p @click="registrationFlag=false" v-show="registrationFlag==true" class="mb-0">You have an account? <a href="#!" class="text-white-50 fw-bold">Login</a>
                </p>
              </div>

            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

</template>

<style scoped>

</style>
