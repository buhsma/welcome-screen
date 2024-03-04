<script>
import myHeader from './components/Header.vue'
import myCard from './components/Card.vue'
import myFooter from './components/FooterMain.vue'

export default {
    data() {
        return {
            date: ''
        }
    },
    components: {
        myHeader,
        myCard,
        myFooter
    },
    methods: {
        getNow: function() {
            const today = new Date();
            const day = String(today.getDate()).padStart(2, '0');
            const month = String(today.getMonth() + 1).padStart(2, '0');
            const year = today.getFullYear();
            const date = day + '.' + month + '.' + year;
            return date;
            }
    },
    mounted() {
        this.date = this.getNow();
    }
}
</script>

<template>

<myHeader :date="date"/>
<myCard :date="date"/>
<myFooter/>

</template>