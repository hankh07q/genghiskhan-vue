<template>
    <div>
      <button v-for="choice in choices" :key="choice.id" @click="$emit('choose', choice.id)">
        {{ choice.choice_text }}
      </button>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      choices: {
        type: Array,
        required: true
      }
    }
  };
  </script>
  