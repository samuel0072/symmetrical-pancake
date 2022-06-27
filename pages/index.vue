<template>
  <div class="container">
    <h1>Sorteios</h1>
    <NuxtLink to="novoSorteio">
      Novo Sorteio
    </NuxtLink>
    <ul>
        <li v-for="(sortition, index) in sortitions" :key="index">
            {{sortition.nickname}}
            <NuxtLink :to="'sorteio/' + sortition.id">
                Visualizar
            </NuxtLink>
        </li>
    </ul>
  </div>
</template>

<script>
const axios = require('axios');

export default {
    name: 'IndexPage',
    data() {
        return {
            sortitions: []
        };
    },
    mounted() {
      this.getAllSortitions();
    },
    methods: {
        getAllSortitions() {
            axios.get("http://127.0.0.1:8002/api/sortition/all")
                .then(res => {
                    this.sortitions = res.data;
                });
        }
    }
}
</script>
