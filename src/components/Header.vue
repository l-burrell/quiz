<template>
  <div id="header-container">
      <div id="counter-box">
        <h2>Quiz App</h2>
        <h4>Counter: {{currentIndex + 1}} out of {{getTotal}}</h4>
      </div>
    <!-- <h4 id="correct-box">Correct: {{numCorrect}} out of {{getTotal}}</h4> -->
    <hr/>
  </div>
</template>

<script>
export default {
 props: {
     currentIndex: Function,
     getTotal: Function,
     numCorrect: Number,
 }
}

</script>
<style scoped>
h2, h4 {
    display: inline-block;
    font-size: 15px;
    margin: 20px 20px 5px 20px;
}
hr {
    color: hsl(120, 20%, 98%);
}
#counter-box {
    display: inline-block;
}
#header-container {
    display: flex;
    justify-content: space-between;
}
</style>