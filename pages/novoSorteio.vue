<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col-md-12">
                <h1 class="h1">Novo Sorteio</h1>
            </div>

        </div>
        <div class="row">
            <div class="col-md-5">
                <button @click="createSortition" class="btn btn-success">Criar</button>
            </div>
        </div>


        <form>
            <div class="row">
                <div class="col-md-6">
                    <div class="form-group">
                        <label for="nickname">Nome do Sorteio</label>
                        <input v-model="sortition.nickname"
                               type="text"
                               id="nickname" class="form-control"/>
                    </div>

                </div>
                <div class="col-md-6">
                    <div class="form-group">
                        <label for="description">Descrição</label>
                        <input
                            v-model="sortition.description"
                            type="text"
                            id="description" class="form-control"/>
                    </div>
                </div>
            </div>

        </form>

        <div class="row">
            <h2 class="h2">Entradas</h2>
            <div class="col-md-6">
                <ul class="list-group">
                    <li
                        v-for="(entry, index) in sortition.entries"
                        :key="index"
                        class="list-group-item">
                        {{entry.value}}
                    </li>
                    <li class="list-group-item">
                        <div class="input-group">
                            <input
                                v-model="newEntry.value"
                                type="text"
                                class="form-control">
                            <button
                                @click="addEntry"
                                class="btn btn-outline-success input-group-text">Adicionar</button>
                        </div>

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
            axios.post("http://127.0.0.1:8000/api/sortition/create", this.sortition)
            .then(res => {
                this.$nuxt.context.redirect("/");
            });
        }
    }
}
</script>
