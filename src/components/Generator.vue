<template>
    <div class="generator">
        <Loader v-if="loading"/>
        <slot v-if="!started">
            <button class="button button--secondary generator__start" v-on:click="generate()">Generate Cocktail</button>
        </slot>
        <div class="cocktail" v-if="drink">
            <h2>Relax with a</h2>
            <h1>{{drink.strDrink}}</h1>
        </div>
    </div>
</template>

<script>
    import Loader from "./Loader";
    import axios from "axios";

    export default {
        name: 'Generator',
        components: {Loader},
        data: function () {
            return {
                started: false,
                loading: false,
                drink: null,
            };
        },
        methods: {
            sleeper(ms) {
                return function (x) {
                    return new Promise(resolve => setTimeout(() => resolve(x), ms));
                };
            },

            generate() {
                // Set started & loading to true
                this.started = true;
                this.loading = true;

                // Get the random cocktail
                // Cocktails provided by https://www.thecocktaildb.com
                axios.get('https://www.thecocktaildb.com/api/json/v1/1/random.php').then(function (response) {
                    setTimeout(function () {
                        this.drink = response.data.drinks[0];
                        console.log(this.drink);
                        this.loading = false
                    }.bind(this), 2000);
                }.bind(this))

            }
        }
    }
</script>
