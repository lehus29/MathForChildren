<template>
    <div 
      v-for="post in posts" 
      :key="post.id" 
      class="question d-flex flex-column align-items-center mt-3"
    >
        <div 
          class="question__text" 
          :class="{ 'fs-4': windowWidth >  400, 'fs-5': windowWidth <=  400 }"
        >{{ post.question }}</div>

        <div class="question__image position-absolute">
            <img 
              class="question__image-img" 
              :src="post.image.url" 
              :alt="post.image.alt"
            >
        </div>
    </div>
</template>
  
<script>
export default {
    name: "TheQuestionType",
    
    props: {
      posts: Array,
      windowWidth: Number
    }
};
</script>

<style scoped>
.question__image {
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
</style>