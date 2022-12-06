<template>
  <div class="w-full h-full flex justify-center items-center">
    <div class="flex flex-col" v-if="state.delivered">
      <div class="text-2xl">{{ state.delivery }}</div>
      <button
        role="button"
        name="another"
        class="border-2 rounded hover:bg-sky-700 hover:text-white"
        @click="state.delivered = false"
        :disabled="state.loading"
      >
        Another!
      </button>
    </div>
    <div class="flex flex-col" v-else>
      <div class="text-2xl">{{ state.setup }}</div>
      <button
        role="button"
        name="tell"
        class="border-2 rounded hover:bg-sky-700 hover:text-white"
        @click="state.delivered = true"
        :disabled="state.loading"
      >
        Tell Me!
      </button>
    </div>
  </div>
</template>

<script setup>
import { reactive, onMounted, watch, nextTick } from 'vue'
const state = reactive({
  setup: null,
  delivery: null,
  delivered: false,
})

const getJoke = async () => {
  try {
    let jokeQuery = await fetch('https://v2.jokeapi.dev/joke/christmas')
    let jokeResponse = await jokeQuery.json()
    state.setup = jokeResponse.setup
    state.delivery = jokeResponse.delivery
  } catch (e) {
    alert('There was an API error')
  }
}

onMounted(async () => {
  await getJoke()
})

watch(
  () => state.delivered,
  async newDelivered => {
    if (newDelivered == true) state.setup = 'Loading Joke...'
    if (newDelivered == false) await getJoke()
  }
)
</script>
