<template>
  <div id="app">
    <p>
      Clicked: {{ count }} times
    </p>
    <p>
      count is {{ evenOrOdd }}.
    </p>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementIfOdd">Increment if odd</button>
    <button @click="incrementAsync">Increment async</button>
  </div>
</template>

<script>
import { computed } from 'vue'
import { useStore } from 'vuex'

export default {
  setup () {
    const store = useStore()

    return {
      count: computed(() => store.state.count),
      evenOrOdd: computed(() => store.getters.evenOrOdd),
      increment: () => store.dispatch('increment'),
      decrement: () => store.dispatch('decrement'),
      incrementIfOdd: () => store.dispatch('incrementIfOdd'),
      incrementAsync: () => store.dispatch('incrementAsync')
    }
  }
}
</script>