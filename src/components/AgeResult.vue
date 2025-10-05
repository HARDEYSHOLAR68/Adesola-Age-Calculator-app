<script setup>
const props = defineProps({
  result: {
    type: Object,
    required: true,
    default: () => ({
      years: '--',
      months: '--',
      days: '--'
    })
  }
})


</script>

<template>
    <div class="result">
    <h1><span id="yy">{{ result.years }}</span>years</h1>
        <h1><span id="yy">{{ result.months }}</span>months</h1>
        <h1><span id="yy">{{ result.days }}</span>days</h1>
    </div>
    
</template>