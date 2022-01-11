<template>
    <div class="section">
        <div class="columns">
            <div class="column">
                <div class="box">
                    <div class="list">
                        <div class="list-item is-justify-content-center" v-for="character in characters" :key="character.id">
                            <div class="list-item-image my-5">
                                <figure class="image is-128x128">
                                    <img class="is-rounded" :src="character.image">
                                </figure>
                            </div>
                            <div class="list-item-content">
                                <div class="list-item-title">
                                    {{character.name}}
                                </div>
                                <div class="list-item-description">
                                    Espécie: {{character.species}}
                                    <br>
                                    Status: {{character.status}}
                                    <br>
                                    Gênero: {{character.gender}}
                                    <br>
                                    Localização: {{character.location.name}}
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { eventBus } from './../main'

import 'bulma-list/css/bulma-list.css'

export default {
    name: 'CharacterInfo',
    props: {
        characters: {
            type: Array,
            required: true
        },
        nextPage: {
            type: String,
            default: ''
        },
        loading: {
            type: Boolean
        }
    },
    created() {
        document.addEventListener('scroll', () => {
            if (window.scrollY + window.innerHeight >= document.body.clientHeight - 400) {
                if (this.nextPage) {
                    if (!this.loading) {
                        eventBus.$emit('nextPageCharacter', this.nextPage)
                    }
                }
            }
        })
    }
}
</script>