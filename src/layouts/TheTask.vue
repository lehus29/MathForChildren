<template>
    <div class="nav__text fw-normal">{{taskCondition}}</div>
</template>
  
<script>
export default {
    name: "TheTask",
    
    props: {
        taskCondition: String
    }
};
</script>

<style scoped></style>