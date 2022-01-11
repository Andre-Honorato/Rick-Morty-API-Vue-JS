<template>
    <section class="section">
        <div class="columns is-centered">
            <div class="column is-6">
                <div class="field">
                    <label class="label">Nome do personagem</label>
                    <div class="control">
                        <input class="input" type="text" placeholder="Ex: Rick Sanchez" autofocus @keyup.enter="searchCharacter" v-model="characterName">
                    </div>
                </div>
                <div class="buttons is-left">
                    <button class="button is-success" @click="searchCharacter">Procurar</button>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import { eventBus } from './../main'

export default {
    name: 'CharacterInput',
    data() {
        return {
            characterName: ''
        }
    },
    methods: {
        searchCharacter() {
            eventBus.$emit('searchCharacter', this.characterName)
        }
    }
}
</script>