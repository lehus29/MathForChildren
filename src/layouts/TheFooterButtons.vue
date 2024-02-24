<template>
  <div class="footer fixed-bottom w-100">
    <div 
      ref="answerMessage" 
      class="footer__answerMessage fs-4 text-center"
    ></div>

    <div class="footer__answers-wrapper d-flex align-items-center p-4 gap-4">
        <FooterAnswerButton
            v-for="(choice, index) in choicesArr"
            :key="index"
            :answer="choice"           
            @button-clicked="saveAnswer"
            ref="answerButtons"
        ></FooterAnswerButton>
        
        <FooterCheckButton 
          ref="checkButton" 
          @click="checkResult" 
          :style="{ pointerEvents: 'none' }"
        ></FooterCheckButton>
    </div>    
  </div>
</template>

<script>
import FooterAnswerButton from "@/components/FooterAnswerButton.vue";
import FooterCheckButton from "@/components/FooterCheckButton.vue";

export default {
  name: "TheFooterButtons",
  
  components: {
    FooterAnswerButton,
    FooterCheckButton,
  },

  props: {
    choicesArr: Array,
    correctAnswer: String
  },

  data() {
    return {
      selectedChoice: null,
    };
  },

  methods: {
    saveAnswer(answer) {
      this.$refs.answerButtons.forEach(button => {
        button.$el.classList.remove('answer_button_selected');
      });

      const clickedButton = this.$refs.answerButtons.find(button => {
        return button.$el.getAttribute('data-answer') === answer;
      });
      if (clickedButton) {
        clickedButton.$el.classList.add('answer_button_selected');
        this.selectedChoice = answer;
      }

      this.$refs.checkButton.$el.style.pointerEvents = 'auto';
    },

    checkResult() {
      const answerBtn = this.$refs.answerButtons.find(button => {
        return button.$el.getAttribute('data-answer') === this.selectedChoice;
      });
      const correctAnswerBtn = this.$refs.answerButtons.find(button => {
        return button.$el.getAttribute('data-answer') === this.correctAnswer;
      });

      this.$refs.answerButtons.forEach(button => {
        button.$el.style.pointerEvents = 'none';
      });

      this.$refs.answerMessage.classList.add('pt-3');
      this.$refs.answerMessage.style.color = this.correctAnswer === this.selectedChoice ? 'green' : 'red';
      this.$refs.answerMessage.textContent = this.correctAnswer === this.selectedChoice ? 'Все верно!' : 'Ничего страшного)';

      this.$refs.checkButton.$el.style.pointerEvents = 'none';

      if (answerBtn) {
        answerBtn.$el.classList.add(this.correctAnswer === this.selectedChoice ? 'correctGreen' : 'incorrect');
      }
      if (correctAnswerBtn !== answerBtn) {
        correctAnswerBtn.$el.classList.add('correctLightGreen');
      }

      setTimeout(() => {
          this.$refs.answerButtons.forEach(button => {
              button.$el.style.pointerEvents = 'auto';
              button.$el.classList.remove('correctGreen', 'incorrect');
          });
          this.$refs.checkButton.$el.style.pointerEvents = 'none';
          correctAnswerBtn.$el.classList.remove('correctLightGreen');
          answerBtn.$el.classList.remove('answer_button_selected');
          this.$refs.answerMessage.textContent = '';
          this.$refs.answerMessage.classList.remove('pt-3');
          this.selectedChoice = null;
      },  3500);
    },
  }
};
</script>

<style scoped>
.footer {
  background-color: rgb(240, 240, 240);
}

.answer_button_selected {
    background-color: #D3D4D5;
    border: #c6c7c8;
}

.correctGreen {
    background-color: green;
}

.incorrect {
    background-color: red;
}

.correctLightGreen {
    background-color: #45ff45;
    border: #c6c7c8;
}
</style>



