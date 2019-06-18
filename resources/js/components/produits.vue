<template>
    <div>
        <input type="text" v-model="fruit" placeholder="ajouter un fruit"/>
        <button v-on:click="ajouterFruit">Ajouter !</button>
        <ul>
            <li v-for="(fruit, index) in fruits">
                {{fruit.nom}}
                <button v-on:click="supprimerFruit(index)">Supprimer</button>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                fruit: '',
                fruits: [
                    {
                        nom: "tomate"
                    },
                    {
                        nom: "cacao"
                    },
                    {
                        nom: "banane"
                    },
                    {
                        nom: "pomme"
                    },
                ]
            }
        },
        watch: {
            fruits: function (val) {
                if(val.length > 0){
                    sessionStorage.setItem('fruits', JSON.stringify(val));
                }
                else sessionStorage.removeItem('fruits');
            },
        },
        methods: {
            ajouterFruit: function () {
                let vm = this;
                vm.fruits.push({nom: vm.fruit});

            },
            supprimerFruit: function(index){
                let vm = this;
                vm.fruits.splice(index, 1);
            },
            getFruits : function () {
                let vm = this;
                if (window.sessionStorage) {
                    let fruits;
                    try {
                        fruits = JSON.parse(sessionStorage.fruits)
                        // fruits = JSON.parse(sessionStorage.getItem('fruits'))
                    }
                    catch (e) {
                        fruits = vm.fruits;
                    }
                    return  fruits;
                }
                else return vm.fruits;
            }
        },
        created() {
            let vm = this;
            vm.fruits = vm.getFruits();
        }
    }
</script>