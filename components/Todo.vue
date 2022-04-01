<template>
  <div class="relative flex items-top justify-center min-h-screen bg-gray-100 sm:items-center sm:pt-0">
    <div class="w-full">
      <!-- component -->
      <div class="h-100 w-full flex items-center justify-center bg-teal-lightest font-sans">
        <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg">
          <div class="mb-4">
            <h1 class="font-bold text-3xl text-gray-600 text-center">Todo List</h1>
            <form class="flex mt-4" @submit.prevent="formSubmit">
              <input class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker" v-model="form.item" placeholder="Add Todo">
              <button class="flex-no-shrink p-2 border-2 rounded-lg text-teal border-teal text-white bg-blue-500 hover:bg-blue-700" type="submit">Add</button>
            </form>
          </div>
          <div>
            <div class="flex mb-4 items-center" v-for="item in items">
              <p :class="[item.status === 'success' ? 'line-through text-green-600' : '', `w-full text-grey-darkest font-semibold text-gray-600`]">{{ item.title }}</p>
              <button class="flex-no-shrink w-1/3 p-2 ml-4 mr-2 border-2 rounded-lg border-grey">Not Done</button>
              <button class="flex-no-shrink p-2 ml-4 mr-2 border-2 rounded-lg border-green text-white bg-green-500 hover:bg-green-700">Done</button>
              <button class="flex-no-shrink p-2 ml-2 border-2 rounded-lg text-red border-red text-white bg-red-500 hover:bg-red-700" @click="removeItem(item.id)">Remove</button>
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: "Todo",
  data() {
    return {
      form: {
        item: null
      },
      items: []
    }
  },
  methods: {
    getRecords() {
      this.$axios.get('/todos').then(response => {
        this.items = response.data.items
      }).catch(error => {
        console.log(error)
      })
    },
    formSubmit() {

    },
    removeItem(id) {
      this.$nuxt.$loading.start()
      this.$axios.delete(`/todos/${id}`).then(() => {
        this.getRecords()
      }).catch(error => {
        console.log(error)
      })
    }
  },
  mounted() {
    this.getRecords()
  }
}
</script>

<style scoped>

</style>
