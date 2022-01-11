<template>
    <div>
        <HeroBanner />
        <CharacterInput />
        <div v-if="errorSearch" class="columns is-centered">
            <div class="column">
                <p class="has-text-danger has-text-centered">Personagem não encontrado!</p>
            </div>
        </div>
        <template v-if="characters">
            <CharacterInfo 
                :characters="characters"
                :nextPage="nextPage"
                :loading="loading"
            />
        </template>
    </div>
</template>

<script>
import { eventBus } from './../main'
import axios from 'axios'

import CharacterInput from './CharacterInput.vue'
import CharacterInfo from './CharacterInfo.vue'
import HeroBanner from './HeroBanner.vue'

export default {
    name: 'Home',
    data() {
        return {
            characters: undefined,
            nextPage: undefined,
            errorSearch: false,
            loading: false
        }
    },
    components: {
        CharacterInput,
        CharacterInfo,
        HeroBanner
    },
    created() {
        eventBus.$on('searchCharacter', async (characterName) => {
            try {
                this.loading = true
                const response = await axios.get(`https://rickandmortyapi.com/api/character/?name=${characterName}`)
                this.loading = false
                this.characters = response.data.results
                if (this.nextPage != response.data.info.next) {
                    this.nextPage = response.data.info.next
                }
                else {
                    this.nextPage = undefined
                }
                this.errorSearch = false
            } catch (error) {
                console.log(error)
                this.errorSearch = true
            }
        })
        eventBus.$on('nextPageCharacter', async (characterNextPage) => {
            try {
                this.loading = true
                const response = await axios.get(characterNextPage)
                this.loading = false
                if (this.nextPage != response.data.info.next) {
                    this.nextPage = response.data.info.next
                }
                else {
                    this.nextPage = undefined
                }
                response.data.results.forEach(result => {
                    this.characters.push(result)
                })
            } catch (error) {
                console.log(error)
            }
        })
    }
}
</script>