<script>
    const sheet_id = import.meta.env.VITE_GOOGLE_SHEET_ID;
    const api_token = import.meta.env.VITE_GOOGLE_API_KEY;
  let id = 0;
  export default {
    props: ['date'],
    data() {
        return {
            cards: [],
            // currDate: this.date,
        }
    },
    computed: {
    isHighlighted() {
        for (let i = 0; i < this.cards.length; i++) {
            const card = this.cards[i];
            if (card.config === 'highlight' || this.date === card.dateEvent) {
                return true;
            }
        }
        return false;
    }
},
  methods: {
    async getStuff() {
        let url = `https://sheets.googleapis.com/v4/spreadsheets/${sheet_id}/values:batchGet?ranges=A2%3AE100&valueRenderOption=FORMATTED_VALUE&key=${api_token}`;
        let cards = []
        const stuff = await fetch(url)
        const data = await stuff.json()
        const cardData = data.valueRanges[0].values
        for (let i = 0; i < cardData.length; i++) {
            const element = cardData[i];
            const card = {
                id: id++,
                dateEvent: element[1],
                time: element[0],
                title: element[2],
                adress: element[3],
                config: element[4]
            }
            cards.push(card)
        }
        this.cards = cards;
    },
    isCardHighlighted(card) {
        return card.config === 'highlight' || this.date === card.dateEvent;
    },
    getDaysUntil: function(startDateStr, endDateStr) {
    const startDateParts = startDateStr.split('.');
    const endDateParts = endDateStr.split('.');

    const startDate = parseInt(startDateParts[0]);
    const startMonth = parseInt(startDateParts[1]);
    const startYear = parseInt(startDateParts[2]);

    const endDate = parseInt(endDateParts[0]);
    const endMonth = parseInt(endDateParts[1]);
    const endYear = parseInt(endDateParts[2]);

    const startTimestamp = new Date(startYear, startMonth - 1, startDate).getTime();
    const endTimestamp = new Date(endYear, endMonth - 1, endDate).getTime();

    const difference = Math.abs(endTimestamp - startTimestamp);
    const days = Math.ceil(difference / (1000 * 60 * 60 * 24));

    return days;
    }
  },
  mounted() {
    this.getStuff();
  }
}
</script>

<template>
    <div class="card" v-for="card in cards" :class="{ 'highlighted': isCardHighlighted(card) }">
        <p class="date">{{ card.dateEvent }}</p>
        <div class="time">
            <p>In {{ getDaysUntil(card.dateEvent, date) }} Tagen {{ card.time }} Uhr</p>
            <p class="adress">{{ card.adress }}</p>
        </div>
        <h1>{{ card.title }}</h1>
    </div>
</template>

<style scoped>
    div {
        display: flex;
        flex-direction: column;
        font-size: 0px;
    }
    .time{
        flex-direction: row;
        gap: 35px;
        color: red;
    }
    h1{
        color: bisque;
        font-size: 40px;
        margin: 10px;
    }
    .card {
        padding: 30px;
        margin: 35px;
        background-color: rgb(25, 57, 163);
    }
    p {
        font-size: 30px;
        margin: 10px;
    }
    .adress {
        color: black;
    }
    .highlighted {
        background-color: rgb(219, 166, 20);
    }
</style>