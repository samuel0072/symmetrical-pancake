<template>
    <div class="container mt-3">
        <div>
            <h1 class="h1">Dados do Sorteio</h1>

            <div id="basic-data" class="row">
                <div class="col-md-">

                </div>
                <div class="col-md-5">
                    <input
                        id="sortition-nickname"
                        v-model="sortition.nickname"
                        type="text"
                        class="form-control"
                    >
                </div>
                <div class="col-md-5">
                    <input
                        id="sortition-description"
                        v-model="sortition.description"
                        type="text"
                        class="form-control"
                    >
                </div>

            </div>

            <div class="row mt-2">
                <div class="col-md-4"></div>
                <div class="col-md-5">
                    <div class="btn-group">
                        <button
                            @click="performSaves"
                            class="btn btn-success">Salvar</button>
                        <button @click="performSortition"
                                class="btn btn-warning">Executar Sorteio</button>
                    </div>
                </div>
            </div>

            <div v-if="results.length > 0">
                <div class="row" >
                    <div class="col-md-3">

                    </div>
                    <h3 class="h3">Resultados</h3>
                </div>
                <div class="row">
                    <div class="col-md-3">

                    </div>
                    <div class="col-md-5">
                        <ul
                            class="list-group">
                            <li v-for="(res, index) in results"
                                :key="index" class="list-group-item">
                                {{res.value}}
                            </li>
                        </ul>
                    </div>
                </div>
            </div>

        </div>

        <div>
            <div class="row">
                <div class="col-md-3">

                </div>
                <h2>Entradas Salvas</h2>
                <div class="col-md-5">
                    <ul id="pre-saved-entries"
                        class="list-group">
                        <li
                            v-for="(entry, index) in sortition.entries"
                            :key="index"
                            class="list-group-item">
                            {{entry.value}}
                        </li>
                    </ul>
                </div>

            </div>
            <div class="row">
                <div class="col-md-3">

                </div>
                <h2>Entradas Recém-Informadas</h2>
                <div class="col-md-5">
                    <ul id="new-entries" class="list-group">
                        <li
                            v-for="(entry, index) in newEntries"
                            :key="index" class="list-group-item">
                            {{entry.value}}
                        </li>
                        <li class="list-group-item">
                            <div class="input-group">
                                <input type="text" v-model="newEntry.value"
                                       class="form-control"/>
                                <button @click="addEntry"
                                        class="btn btn-outline-success input-group-text">Adicionar</button>
                            </div>

                        </li>
                    </ul>
                </div>

            </div>
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
                entries: [],
                description: "Placeholder",
                type: "Placeholder",
                performed: false,
                created_at: "",
                updated_at: "",
                deleted_at: ""
            },
            newEntries: [],
            newEntry: {
                value:""
            },
            results: []

        }
    },
    mounted() {
        this.getSortition(this.$route.params.id);
    },
    methods: {
        getSortition(id) {
            axios.get(`http://127.0.0.1:8000/api/sortition/${id}`)
                .then(res => {
                    this.sortition = res.data;
                });
        },
        addEntry() {
            this.newEntries.push({
                value: this.newEntry.value,
            });
        },
        saveNewEntries() {
            const data = {
                entries: this.newEntries,
                sortitionId: this.sortition.id,

            };
            axios.post("http://127.0.0.1:8000/api/sortition/addEntries", data)
                .then(res => {
                    this.sortition.entries = this.sortition.entries.concat(res.data);
                    this.newEntries = [];
                }).catch(e => {});


        },
        performSortition() {
            const id = this.sortition.id;
            const data = {
                options: {},
                entries: this.newEntries
            };
            axios.post(`http://127.0.0.1:8000/api/sortition/execute/${id}`, data)
            .then(res => {
                this.results = this.results.concat(res.data);
            });
        },
        saveSortitionData() {
            try {
                axios.put("http://127.0.0.1:8000/api/sortition/edit", this.sortition);
            } catch (e) {

            }

        },
        performSaves() {
            this.saveNewEntries();
            this.saveSortitionData();
        }
    },


}
</script>
