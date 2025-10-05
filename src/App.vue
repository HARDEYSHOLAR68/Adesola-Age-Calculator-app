<script setup>
import { ref } from 'vue'
import AgeForm from './components/AgeForm.vue'
import AgeResult from './components/AgeResult.vue'

const result = ref({ years: '--', months: '--', days: '--' })

function calculateAge({ day, month, year }) {
  const today = new Date()
  const birthDate = new Date(year, month - 1, day)

  let hasError = false

  // Validate inputs
  if (!day || day < 1 || day > 31) {
    document.getElementById('dayError').textContent = 'Enter a valid day (1-31)'
    hasError = true
  } else {
    document.getElementById('dayError').textContent = ''
  }

  if (!month || month < 1 || month > 12) {
    document.getElementById('monthError').textContent = 'Enter a valid month (1-12)'
    hasError = true
  } else {
    document.getElementById('monthError').textContent = ''
  }

  if (!year || year > today.getFullYear()) {
    document.getElementById('yearError').textContent = 'Enter a valid year'
    hasError = true
  } else {
    document.getElementById('yearError').textContent = ''
  }

  if (hasError) return

  if (birthDate > today) {
    document.getElementById('yearError').textContent = 'Date of birth cannot be in the future!'
    return
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
      <!-- Correct event name -->
      <AgeForm @on-submit="calculateAge" />

      <!-- Pass result to child -->
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

p[id$="Error"] {
  color: red;
  font-size: 13px;
  margin: 5px 0 0;
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
    .result{
        margin-top: 4rem;
    }
    .result h1{
        font-size: 4rem;
    }
}
</style>