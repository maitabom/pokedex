<template>
    <div class="card mb-3">
        <div class="card-header">
            <img :src="image" @mouseover="toBack()" @mouseout="toFront()" class="card-img-top" :alt="name">
        </div>
        <div class="card-body">
            <h3 class="card-title">{{ nomePokemon }}</h3>
            <h4 class="card-subtitle mb-2 text-body-secondary">{{ type }}</h4>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'Pokemon',
    props: {
        name: String,
        url: String,
        num: Number
    },
    data() {
        return {
            image: '',
            type: '',
            pokemon: {}
        }
    },
    computed: {
        nomePokemon() {
            var nome = this.name;
            var newName = nome[0].toUpperCase() + nome.slice(1);
            return newName;
        }
    },
    created: function () {
        axios.get(this.url).then(response => {
            this.pokemon = response.data;
            this.image = response.data.sprites.front_default;
            this.type = response.data.types[0].type.name;
        });
    },
    methods: {
        toBack: function() {
            this.image = this.pokemon.sprites.back_default;
        },
        toFront: function() {
            this.image = this.pokemon.sprites.front_default;
        }
    }
}
</script>

<style scoped></style>