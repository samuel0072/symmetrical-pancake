<template>
    <div>
        <div v-if="sortition !== null">
            {{sortition.nickname}}
        </div>
    </div>
</template>
<script>
const axios = require('axios');

export default {

    data() {
        return {
            sortition: {
                nickname: "Placeholder",
            },
        }
    },
    methods: {
        getSortition(id) {
            axios.get(`http://127.0.0.1:8000/api/sortition/${id}`)
                .then(res => {
                    this.sortition = res.data;
                });

        }
    },
    mounted() {
        this.getSortition(this.$route.params.id);
    }


}
</script>
