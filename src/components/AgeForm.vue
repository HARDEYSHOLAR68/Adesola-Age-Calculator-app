<script setup>
import { ref } from 'vue'

const day = ref('')
const month = ref('')
const year = ref('')

const dayError = ref('')
const monthError = ref('')
const yearError = ref('')

const emit = defineEmits(['on-submit'])

// Limit input max values
function enforceMaxValue(event, maxValue, refValue) {
  if (Number(event.target.value) > maxValue) {
    event.target.value = maxValue
    refValue.value = maxValue
  }
}

// Handle form submit
function handleSubmit(e) {
  e.preventDefault()

  // clear previous errors
  dayError.value = ''
  monthError.value = ''
  yearError.value = ''

  // get values
  const dayNum = Number(day.value)
  const monthNum = Number(month.value)
  const yearNum = Number(year.value)
  const currentYear = new Date().getFullYear()

  let hasError = false

  // validation
  if (!dayNum || !monthNum || !yearNum) {
    if (!dayNum) dayError.value = 'Please fill in the day'
    if (!monthNum) monthError.value = 'Please fill in the month'
    if (!yearNum) yearError.value = 'Please fill in the year'
    return
  }

  if (dayNum < 1 || dayNum > 31) {
    dayError.value = 'Please enter a valid day between 1-31'
    hasError = true
  }

  if (monthNum < 1 || monthNum > 12) {
    monthError.value = 'Please enter a valid month between 1-12'
    hasError = true
  }


  if (String(year.value).length !== 4) {
    yearError.value = 'Year must be 4 digits (e.g., 1998)'
    hasError = true
  } else if (yearNum > currentYear) {
    yearError.value = 'Enter a valid year (not in the future)'
    hasError = true
  }

  if (hasError) return

  // Emit only after validation passes
  emit('on-submit', { day: dayNum, month: monthNum, year: yearNum })
}
</script>

<template>
  <div>
    <form @submit="handleSubmit" id="ageForm">
      <div class="form_container">
        <div class="block">
          <label for="day">Day</label>
          <input
            v-model.number="day"
            type="number"
            id="day"
            placeholder="DD"
            min="1"
            max="31"
            @input="enforceMaxValue($event, 31, day)"
            class="border"
          />
          <transition name="fade">
            <p v-if="dayError" id="dayError" class="error">{{ dayError }}</p>
          </transition>
        </div>

        <div class="block">
          <label for="month">Month</label>
          <input
            v-model.number="month"
            type="number"
            id="month"
            placeholder="MM"
            min="1"
            max="12"
            @input="enforceMaxValue($event, 12, month)"
            class="border"
          />
          <transition name="fade">
            <p v-if="monthError" id="monthError" class="error">{{ monthError }}</p>
          </transition>
        </div>

        <div class="block">
          <label for="year">Year</label>
          <input
            v-model.number="year"
            type="number"
            id="year"
            placeholder="YYYY"
            :max="new Date().getFullYear()"
            @input="enforceMaxValue($event, new Date().getFullYear(), year)"
            class="border"
          />
          <transition name="fade">
            <p v-if="yearError" id="yearError" class="error">{{ yearError }}</p>
          </transition>
        </div>
      </div>

      <div class="submit_block">
        <hr />
        <button type="submit" class="submit_btn">
          <img src="@/assets/images/icon-arrow.svg" alt="icon" />
        </button>
      </div>
    </form>
  </div>
</template>