<template>
    <section class="score">
      Player <span>{{ winCount }}</span> x <span>{{ loseCount }}</span> Computer
    </section>
  </template>
  
  <script>
  export default {
    name: 'ScoreBoard',
    props: {
      winCount: {
        type: Number,
        required: true
      },
      loseCount: {
        type: Number,
        required: true
      }
    }
  }
  </script>
  <!-- crete component -->
  <style lang="scss">
  section.score {
    border-bottom: 1px solid black;
    padding: 24px;
    font-size: 18px;
  
    span {
      padding: 8px;
      font-weight: bold;
      border: 1px solid black;
    }
  }
  </style>
  