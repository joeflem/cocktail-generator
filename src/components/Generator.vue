<template>
    <div class="generator">
        <Loader v-if="status === 'loading'"/>
        <button :class="status ? 'generator__start--active button--primary' : 'button--secondary'"
                class="button generator__start" v-on:click="generate()">Generate Cocktail
        </button>
        <div class="cocktail" v-if="drink && status !== 'loading'">
            <figure class="cocktail__image" :style="{ backgroundImage: `url(${drink.strDrinkThumb})` }"/>
            <h2>Relax with a</h2>
            <h1>{{drink.strDrink}}</h1>

            <div class="cocktail__overview" v-if="status === 'overview'">
                <h4>Category</h4>
                <p>{{drink.strCategory}}</p>

                <h4>Glass Type</h4>
                <p>{{drink.strGlass}}</p>

                <h4>Type</h4>
                <p>{{drink.strAlcoholic}}</p>

                <button v-on:click="showHow()" class="button button--primary">Show me how to make it</button>
            </div>

            <div class="cocktail__method" v-if="status === 'method'">
                <ul class="cocktail__method__ingredients">
                    <li v-for="ingredient in drinkIngredients" :key="ingredient.id">
                        <p><strong>{{ingredient.measurement}}</strong> {{ingredient.ingredient}}</p>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    import Loader from "./Loader";
    import axios from "axios";
    import _ from "lodash"; // eslint-disable-line

    export default {
        name: 'Generator',
        components: {Loader},
        data: function () {
            return {
                status: null,
                drink: null,
                drinkIngredients: [],
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
                this.status = 'loading';
                // Get the random cocktail
                // Cocktails provided by https://www.thecocktaildb.com
                axios.get('https://www.thecocktaildb.com/api/json/v1/1/random.php').then(function (response) {
                    setTimeout(function () {
                        this.drink = response.data.drinks[0];
                        console.log(this.drink);

                        // Add ingredients and methods into a nicer format
                        [...Array(15)].map((_, count) => { // loop 15 times
                            // Get ingredients and measurements
                            let ingredient = this.drink['strIngredient' + count];
                            let measurement = this.drink['strMeasure' + count];

                            // Create a new array from the data
                            if (ingredient && measurement) {
                                this.drinkIngredients.push({
                                    'id': count,
                                    'ingredient': ingredient,
                                    'measurement': measurement,
                                })
                            }
                        });

                        this.status = 'overview';
                    }.bind(this), 2000);
                }.bind(this));
            },

            showHow() {
                this.status = 'method';
            }
        }
    }
</script>
