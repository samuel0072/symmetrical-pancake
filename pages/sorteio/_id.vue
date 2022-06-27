<template>
    <div>
        <div id="basic-data">
            <div>
                <h1>{{sortition.nickname}}</h1>
                <p>{{sortition.description}}</p>
            </div>

            <div>
                <button @click="saveNewEntries">Salvar</button>
                <button @click="performSortition">Executar Sorteio</button>
            </div>
            <div v-if="results.length > 0">
                <h3>Resultados</h3>
                <ul v-for="(res, index) in results" :key="index">
                    <li>
                        {{res.value}}
                    </li>
                </ul>
            </div>

        </div>
        <div>
            <div>
                <h2>Entradas salvas</h2>
                <ul id="pre-saved-entries">
                    <li v-for="(entry, index) in sortition.entries" :key="index">
                        {{entry.value}}
                    </li>
                </ul>
            </div>
            <div>
                <h2>Recém-informadas</h2>
                <ul id="new-entries">
                    <li v-for="(entry, index) in newEntries" :key="index">
                        {{entry.value}}
                    </li>
                    <li>
                        <input type="text" v-model="newEntry.value"/>
                        <button @click="addEntry">Adicionar</button>
                    </li>
                </ul>
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
            axios.get(`http://127.0.0.1:8002/api/sortition/${id}`)
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
            axios.post("http://127.0.0.1:8002/api/sortition/addEntries", data)
            .then(res => {
                this.sortition.entries = this.sortition.entries.concat(res.data);
                this.newEntries = [];
            });
        },
        performSortition() {
            const id = this.sortition.id;
            const data = {
                options: {},
                entries: this.newEntries
            };
            axios.post(`http://127.0.0.1:8002/api/sortition/execute/${id}`, data)
            .then(res => {
                this.results = this.results.concat(res.data);
            });
        }
    },


}
</script>
