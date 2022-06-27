<template>
  <div class="container mt-2">
      <div class="row">
          <div class="col-md-2">

          </div>
          <div class="col-md-5">
              <h1>Sorteios</h1>
          </div>
          <div class="col-md-2">
              <NuxtLink to="novoSorteio" class="btn btn-outline-success">
                  Novo Sorteio
              </NuxtLink>
          </div>
      </div>
      <div class="row">
          <div class="col-md-2">

          </div>
          <div class="col-md-5">
              <ul  class="list-group" >
                  <li
                      v-for="(sortition, index) in sortitions"
                      :key="index"
                      class="list-group-item clearfix" >
                      {{sortition.nickname}}
                      <NuxtLink :to="'sorteio/' + sortition.id"
                                class="btn btn-warning float-end">
                          Visualizar
                      </NuxtLink>
                  </li>
              </ul>
          </div>

      </div>


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
            axios.get("http://127.0.0.1:8000/api/sortition/all")
                .then(res => {
                    this.sortitions = res.data;
                });
        }
    }
}
</script>
