<script setup>
    import {ref} from 'vue'

    const day = ref ('');
    const month = ref ('');
    const year = ref ('');

    const emit = defineEmits(['on-submit'])

    function handleSubmit (e) {
        e.preventDefault()
        emit('on-submit',{day: Number(day.value), 
            month:Number(month.value), 
            year:Number(year.value)})

            if (!dayNum || !monthNum || !yearNum) {
    alert('Please fill in all fields!')
    return
  }

  if (dayNum < 1 || dayNum > 31) {
    alert('Please enter a valid day between 1 and 31')
    return
  }

  if (monthNum < 1 || monthNum > 12) {
    alert('Please enter a valid month between 1 and 12')
    return
  }

  if (yearNum > new Date().getFullYear()) {
    alert('Please enter a valid year (not in the future)')
    return
  }

  emit('on-submit', { day: dayNum, month: monthNum, year: yearNum })
    }
</script>

<template>
    <div>
        <form @submit="handleSubmit" id="ageForm">
            <div class="form_container">
            <div class="block">
                <label for="day">Day</label>
                <input v-model.number="day" type="number" id="day" placeholder="DD" min="1" max="31" class="border "/>
                <p id="dayError"></p>
            </div>
            
            <div class="block">
                <label for="month">Month</label>
                <input v-model.number="month" type="number" id="month" placeholder="MM" min="1" max="12" class="border"/>
                <p id="monthError"></p>
            </div>
            
            <div class="block">
                <label for="year">Year</label>
                <input v-model.number="year" type="number" id="year" placeholder="YYYY" :max="new Date().getFullYear()" class="border"/>
                <p id="yearError"></p>
            </div>
            
            </div>
            <div class="submit_block">
            <hr>
            <button type="submit" class="submit_btn"><img src='@/assets/images/icon-arrow.svg' alt="icon"></button>
            </div>
        </form>
    </div>

</template>


<style>
/* form{
    display: flex;
} */

</style>