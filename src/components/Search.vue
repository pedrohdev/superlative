<template>
    <div class="mt-4">
        <div align="center" class="row">
            <div class="col-sm">
                <h3>
                    Encontre o superlativo de um adjetivo em inglês
                </h3>
            </div>
        </div>
        <div  class="row mt-4">
            <div  class="col-sm">
                <input v-model="adjective" :disabled="loading == true" type="text" class="form-control" id="adjective" placeholder="Adjetivo">
            </div>
        </div>
        <div class="row mt-2">
            <div class="col-sm">
                <button @click="clicked" :disabled="loading == true" style="width: 100%;" type="button" class="btn btn-danger">Encontrar</button>
            </div>
        </div>
        <div class="row mt-3">
            <div v-html="alert" class="col-sm">
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { syllable } from 'syllable'

export default {
    name: 'Search',
    data(){
        return {
            adjective: '',
            alert: '',
            loading: false
        }
    },
    methods: {
        clicked(){
            this.adjective = this.adjective.toLowerCase()
            if(this.adjective != '' && this.adjective.trim(' ').length != 0){
                this.alert = ''
                this.loading = true
                axios.get(`https://api.dictionaryapi.dev/api/v2/entries/en_US/${this.adjective}`).then(data => {
                    this.loading = false
                    let words = data.data

                    words = words.filter(word => {
                        let meanings = word.meanings
                        meanings = meanings.filter(meaning => {
                            return meaning.partOfSpeech == 'adjective'
                        })

                        return meanings.length != 0
                    })
                    
                    if(words.length != 0){
                        let syllables = syllable(this.adjective)
                        if(syllables <= 2){
                            if(this.adjective == "good"){
                               this.alert = `
                                    <div class="alert alert-success" role="alert">
                                        <strong class="lead">The best</strong>
                                    </div>
                                `  
                            }else if(this.adjective == "bad"){
                                this.alert = `
                                    <div class="alert alert-success" role="alert">
                                        <strong class="lead">The worst</strong>
                                    </div>
                                `
                            }else if(this.adjective[this.adjective.length - 1] == 'e'){
                               this.alert = `
                                    <div class="alert alert-success" role="alert">
                                        <strong class="lead">The ${this.adjective}st</strong>
                                    </div>
                                ` 
                            }else if(this.adjective[this.adjective.length - 1] == 'y' && this.adjective[this.adjective.length - 2].search(/[^aeiou]+/gm) != -1){
                                this.alert = `
                                    <div class="alert alert-success" role="alert">
                                        <strong class="lead">The ${this.adjective.slice(0, -1)}iest</strong>
                                    </div>
                                `
                            }else if(this.adjective[this.adjective.length - 3].search(/[^aeiou]+/gm) != -1 && this.adjective[this.adjective.length - 2].search(/[aeiou]+/gm) != -1 && this.adjective[this.adjective.length - 1].search(/[^aeiou]+/gm) != -1 &&
                            this.adjective.slice(-2) != "er" && this.adjective.slice(-2) != "ly" && this.adjective.slice(-2) != "ow" && this.adjective.slice(-4) != "some"
                            ){
                                this.alert = `
                                    <div class="alert alert-success" role="alert">
                                        <strong class="lead">The ${this.adjective}${this.adjective[this.adjective.length - 1]}est</strong>
                                    </div>
                                `
                            }else if(this.adjective.slice(-2) == "er" || this.adjective.slice(-2) == "ly" || this.adjective.slice(-2) == "ow" || this.adjective.slice(-4) == "some"){
                               this.alert = `
                                    <div class="alert alert-success" role="alert">
                                        <strong class="lead">The ${this.adjective}est ou The most ${this.adjective}</strong>
                                    </div>
                                `
                            }else if(syllables == 2){
                                this.alert = `
                                    <div class="alert alert-success" role="alert">
                                        <strong class="lead">The most ${this.adjective}</strong>
                                    </div>
                                `
                            }else{
                                this.alert = `
                                    <div class="alert alert-success" role="alert">
                                        <strong class="lead">The ${this.adjective}est</strong>
                                    </div>
                                `
                            }
                        }else{
                            this.alert = `
                                <div class="alert alert-success" role="alert">
                                    <strong class="lead">The most ${this.adjective}</strong>
                                </div>
                            `
                        }
                    }else{
                        this.alert = `
                            <div class="alert alert-info" role="alert">
                                A palavra não é um adjetivo
                            </div>
                        `
                    }
                }).catch(() => {
                    this.loading = false
                    this.alert = `
                        <div class="alert alert-danger" role="alert">
                            A palavra não existe
                        </div>
                    `
                })
            }else{
                this.alert = `
                <div class="alert alert-warning" role="alert">
                    Uma dadiva dos ninjas
                </div>
                `
            }
        }
    },
    created(){
    }
}
</script>

<style scoped>
</style>