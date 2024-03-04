<script>
export default {
    props: ['date'],
    data() {
        return {
            title: 'Welcome to Opportunity',
            currDate: this.date
    }
  },
}
</script>

<template>
    <h1>{{ title }}</h1>
    <p>{{ currDate }}</p>
</template>

<style scoped>
h1 {
    font-size: 62px;
    color: #323D4A;
    font-weight: 900;
    margin: 35px;
}
p {
    font-size: 62px;
    color: #9AA7B1;
    font-weight: 500;
    margin: 35px;
} 
</style>






