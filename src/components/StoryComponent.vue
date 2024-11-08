<template>
    <div>
      <p>{{ story.text }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      story: {
        type: Object,
        required: true
      }
    }
  };
  </script>
  