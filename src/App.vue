<template>
    <div class="main container-fluid fw-medium">
        <div class="container">

            <div class="row">
                <div class="col">
                    <div class="player float-end mt-4">
                        <div class="player__name fs-5">Alexey</div> 
                        <div class="playerScore-wrapper d-flex align-items-center fs-5">
                            <div class="player__total">1000</div>
                            <img 
                              src="@/assets/coin.png" 
                              class="player__imgCoin ms-1"
                            >
                        </div>
                    </div>
                </div>
            </div>

            <div class="row">
                <div class="col">
                    <div
                      class="nav d-flex justify-content-between align-items-center mt-5" 
                      :class="{ 'fs-5': windowWidth >  400, 'fs-6': windowWidth <=  400 }"
                    >
                        <div 
                          type="btn" 
                          class="btn btn-light p-2 nav__closeBtn"
                        >
                            <img src="@/assets/close_btn.png" alt="">
                        </div>

                        <TheTask :taskCondition="taskCondition"></TheTask>

                        <div 
                          @click="modalShow = !modalShow" 
                          type="btn" 
                          class="nav__lampBtn btn btn-light p-2"
                        >
                            <img src="@/assets/bulb.png" alt="">
                        </div>
                    </div>
                </div>
            </div>

            <div class="row">
                <div class="col">
                    <TheQuestionType 
                      :posts="posts" 
                      :windowWidth="windowWidth"
                    ></TheQuestionType>
                </div>
            </div>

            <div class="row">
                <div class="col">
                    <div 
                      v-if="modalShow" 
                      class="modalWindow position-absolute d-flex align-items-center justify-content-center rounded"
                    >
                        <div 
                          @click="modalShow = !modalShow" 
                          type="btn" 
                          class="modalWindow__btn btn btn-danger p-2"
                        >Close</div>
                    </div>
                </div>
            </div>

        </div>

        <TheFooterButtons :correctAnswer="correctAnswer" :choicesArr="choicesArr"></TheFooterButtons>
    </div>
</template>
  
<script>
import TheFooterButtons from '@/layouts/TheFooterButtons.vue'
import TheQuestionType from '@/layouts/TheQuestionType.vue'
import TheTask from '@/layouts/TheTask.vue'
import axios from 'axios';

export default {
    name: "AppVue",

    components: {
        TheFooterButtons, 
        TheQuestionType,
        TheTask
    },

    data() {
        return {
            windowWidth: window.innerWidth,
            modalShow : false,
            posts: [],
            taskCondition: null,
            correctAnswer: null,
            choicesArr: null
        }
    },

    mounted() {
        axios.get('http://localhost:8181/questions')
        .then(response => {
            this.posts = response.data;

            let firstPost = this.posts[0];
            this.taskCondition = firstPost.taskCond;
            this.correctAnswer = firstPost.correctAnswer;
            this.choicesArr = firstPost.choices;
        })
        .catch(error => {
            console.error(error);
        });
        window.addEventListener('resize', this.handleResize);
    },

    beforeDestroy() {
        window.removeEventListener('resize', this.handleResize);
    },

    methods: {
        handleResize() {
            this.windowWidth = window.innerWidth;
        },
    }
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.nav__closeBtn {
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.16);
}

.nav__lampBtn {
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.16);
}

.modalWindow {
    background-color: rgb(233, 233, 233);
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 2;
    padding: 100px;
}

@media (max-width: 767px) {
    .question__image {
        width: 100%;              
    }
    .question__image-img {
        width: 100%;
    }
}  
</style>