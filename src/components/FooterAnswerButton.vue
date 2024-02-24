<template>
    <div
      type="btn" 
      class="answer_button btn btn-light p-2 text-center w-100 fs-4 fw-semibold" 
      @click="handleClick" :data-answer="answer"
    >
        <div class="answer_button-letter">{{ answer }}</div>
    </div>
</template>
  
<script>
export default {
    name: "FooterAnswerButton",
    
    props: {
        answer: String
    },
    methods: {
        handleClick() {
            this.$emit('button-clicked', this.answer);
        }
    }
}
</script>

<style scoped>
.answer_button {
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.16);
}
</style>