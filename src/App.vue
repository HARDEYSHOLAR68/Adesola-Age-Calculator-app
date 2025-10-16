<script setup>
import { ref } from 'vue'
import AgeForm from './components/AgeForm.vue'
import AgeResult from './components/AgeResult.vue'

const result = ref({ years: '--', months: '--', days: '--' })

function calculateAge({ day, month, year }) {
  const today = new Date()
  const birthDate = new Date(year, month - 1, day)


  if (birthDate > today) {
    yearError = 'Date of birth cannot be in the future!'
    hasError = true
  }

  // Calculate age
  let years = today.getFullYear() - birthDate.getFullYear()
  let months = today.getMonth() - birthDate.getMonth()
  let days = today.getDate() - birthDate.getDate()

  if (days < 0) {
    months--
    const prevMonth = new Date(today.getFullYear(), today.getMonth(), 0)
    days += prevMonth.getDate()
  }

  if (months < 0) {
    years--
    months += 12
  }

  result.value = { years, months, days }
}
</script>

<template>
  <div class="card">
    <div class="container">

      <AgeForm @on-submit="calculateAge" />

     
      <AgeResult :result="result" />
    </div>
  </div>
</template>


<style>
body{
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: hsl(0, 0%, 94%);
}

.card{
  background-color: hsl(0, 100%, 100%);
  display: flex;
  border-radius: 1rem 1rem 5rem 1rem;
}

.container{
  padding: 2rem;
  display: flex;
  flex-direction: column;
}

form{
  display: flex;
  flex-direction: column;
}

.form_container{
  display: flex;
  gap: 2rem;

}

.block{
  display: flex;
  flex-direction: column;
}

.block label{
  text-transform: uppercase;
  font-weight: 600;

}

input{
  width: 100px;
  padding: 5px;
  border-radius: 5px;
  border: solid hsl(0, 0%, 94%);
  display: flex; align-items: center;
  font-size: 1.5rem;
}

input::placeholder{
  font-size: 1.5rem;
  font-weight: 800;
}

.submit_block{
  display: flex;
  align-items: center;

}

.submit_block hr{
  width: 100%;
}

.submit_block .submit_btn{
  border-radius: 50%;
  padding: 1rem;
  background-color: hsl(259, 100%, 65%);
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.result h1{
  font-size: 6rem;
  font-weight: 800;
  height: fit-content;
}
.result span{
  color: hsl(259, 100%, 65%);
}

.fade p {
  color: red;
  font-size: 0.8rem;
  height: 16px; 
  margin-top: 3px;
}


input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input[type=number] {
  -moz-appearance: textfield;
}

@media screen and (max-width:600px) {
  .card{
        width: 360px;
    }

    .container{
        padding: 1rem;   
    }
    .form_container{
        margin-bottom: 5rem;
        gap: 0;
        justify-content: space-between;
    }
    .submit_block{
        position: relative;
    }
    .submit_btn{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
    }

    .error {
      color: red;
      font-size: 0.8rem;
      height: 16px; /* keeps layout stable even if no error */
      margin-top: 3px;
    }

    .result{
        margin-top: 4rem;
    }


    .result h1{
        font-size: 4rem;
    }
}
</style>