<template>
    <div>
        <h1>Novo Sorteio</h1>
        <button @click="createSortition">Criar</button>
        <form>
            <div>
                <label for="nickname">Nome do Sorteio</label>
                <input v-model="sortition.nickname" type="text"  id="nickname"/>
            </div>
            <div>
                <label for="description">Descrição</label>
                <input v-model="sortition.description" type="text"  id="description"/>
            </div>
        </form>
        <div>
            <h2>Entradas</h2>
            <ul>
                <li v-for="(entry, index) in sortition.entries" :key="index">
                    {{entry.value}}
                </li>
            </ul>
            <input v-model="newEntry.value" type="text">
            <button @click="addEntry">Adicionar</button>
        </div>

    </div>
</template>
<script>
const axios = require('axios');

export default {
    data() {
        return {
            sortition: {
                nickname: "",
                description: "",
                entries: []
            },

            newEntry: {
                value:""
            }
        }
    },
    methods: {
        addEntry() {
            this.sortition.entries.push({
                value: this.newEntry.value
            });
        },
        createSortition() {
            axios.post("http://127.0.0.1:8002/api/sortition/create", this.sortition)
            .then(res => {
                this.$nuxt.context.redirect("/");
            });
        }
    }
}
</script>
