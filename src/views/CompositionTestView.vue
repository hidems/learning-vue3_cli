<template>
  <div>
    <h1>Composition Test</h1>
    <p>{{ name }}</p>
    <p>{{ age }}</p>
    <p>{{ sex }}</p>
    <p>{{ book.title }}</p>
    <p>{{ book.author }}</p>
    <p>{{ titleRef }}</p>
    <p>{{ authorRef[1] }}</p>
    <p>{{ authorRef[2] }}</p>
    <button @click="btnClick">Switch Sex</button>
    <p>Computed Total Price: {{ totalPrice }}</p>
    <div>
      <label>Price: </label><input type="number" v-model="item.price" />
    </div>
    <div>
      <label>Number: </label><input type="number" v-model="item.number" />
    </div>
    <div>
      <label>watch: </label><input type="text" v-model="search" />
    </div>
    <div>
      <label>watchEffect: </label><input type="text" v-model="searchEffect" />
    </div>
  </div>
</template>

<script>
import { ref, reactive, toRefs, computed, watch, watchEffect, onMounted } from 'vue'
export default {
  name: "CompositionTestView",
  setup() {
    console.log("setup") // Order setup->created->mounted
    console.log(this) // Undefined

    let name = "Name from Seup" // Variables are not reactive
    const age = 20

    const sex = ref("Male") // refs

    const book = reactive({ // reactive
      title: "Vue 3",
      author: ["Vue", "Vite"]
    })

    const bookToRefs = reactive({ // toRefs for reactrive with spread operator
      titleRef: "Vue 3 to Ref",
      authorRef: ["Vue to Ref", "Vite to Ref"]
    })

    // methods from setup
    const btnClick = e => {
      sex.value = "Female"
      console.log(book.title)
      console.log(e)
    }

    const item = reactive({ // Variable for computed
      price: null,
      number: null
    })

    // Computed from setup
    const totalPrice = computed(() => {
      if(!item.price || !item.number) {
        return 'Please input price and number'
      }
      return item.price * item.number
    })

    const search = ref('')

    // watch from setup
    watch(search, (newValue, prevValue) => {
      console.log(`Watch search: ${search.value}`)
      console.log(`Prev: ${prevValue}`)
      console.log(`New: ${newValue}`)
    })

    const searchEffect = ref('')
    // watchEffect
    watchEffect(() => {
      console.log(`WatchEffect: ${searchEffect.value}`)
    })

    // onMounted
    onMounted(() => {
      console.log("onMounted")
    })

    return { // Variables need to be returned in order to be used in template
      name,
      age,
      sex,
      book,
      ...toRefs(bookToRefs), // Without toRefs, bookToRefs will be not a reactive
      btnClick,
      totalPrice,
      item,
      search,
      searchEffect,
    }
  },
  data() {
    return {
      nameData: "Name from Data",
      ageData: 20
    }
  },
  created() {
    console.log("created")
    console.log(this)
    console.log("Variable from setup: ", this.name)
    console.log("Variable from data: ", this.nameData)
    console.log("Ref from setup: ", this.sex)
    console.log("Reactive from setup: ", this.book.title)
    console.log("Reactive from setup: ", this.book.author)
  },
  mounted() {
    console.log("mounted")
  },
}
</script>

<style>

</style>