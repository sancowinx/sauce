<script setup>
import { defineComponent, ref } from 'vue'

defineComponent({
  name: 'CodeForm'
})

const { VITE_DIGIT_BASE_URL } = import.meta.env

const digit = ref('')
const containsCharacter = ref(false)
const isMetamorphosis = ref(false)

function navigate() {
  window.open(`${VITE_DIGIT_BASE_URL}/${digit.value}`)
}

function onInput(event) {
  if (isNaN(event.target.value)) {
    return
  } else {
    isMetamorphosis.value = event.target.value === '177013'
    containsCharacter.value = false
    digit.value = event.target.value.replace(/[^0-9]/g, '')
  }
}

function onPaste(event) {
  let paste = (event.clipboardData || window.clipboardData).getData("text")
  if (isNaN(paste)) containsCharacter.value = true
  else containsCharacter.value = false
}

function keypress(event) {
  console.log('keypress', event.charCode)
  if (!Boolean((event.charCode >= 48 && event.charCode <= 57) || event.charCode === 13)) event.preventDefault()
}
</script>

<template>
  <h1>Sauce</h1>
  <form>
    <div class="card">
      <input
        type="text"
        inputmode="numeric"
        spellcheck="false"
        autocorrect="off"
        pattern="\d+"
        id="digit"
        name="digit"
        maxlength="6"
        placeholder="6-digit"
        :value="digit"
        @paste="onPaste"
        @keypress="keypress"
        @input="onInput"
      />
    </div>
    <div class="card">
      <button :disabled="!digit || containsCharacter || isMetamorphosis" type="submit" @click="navigate">Go</button>
    </div>
  </form>
  <div v-if="isMetamorphosis">It got removed</div>
</template>

<style scoped>
button{
  width: 100%;
}
</style>
