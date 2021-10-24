<template>
  <div style="text-align: center; margin: 2rem 0;">
    <h1 class="title">Star Wars</h1>
    <p>TypeScript, Services, Composition API, Rest API</p>
  </div>
  <div v-if="loading">
    <Spinner/>
  </div>
  <div v-else>
    <table>
      <!-- header -->
      <thead>
        <tr>
          <td>Name</td>
          <td>Gender</td>
          <td>Mass</td>
        </tr>
      </thead>
      <!-- body -->
      <tbody>
        <tr v-for="(people, index) in peoples.results" :key="index">
          <td>{{ people.name }}</td>
          <td>{{ people.gender }}</td>
          <td>{{ people.mass }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue'
import DataService from '@/services/DataService'
import Spinner from '@/components/UI/Spinner.vue'
import ResponseData from '@/types/ResponseData'
import Peoples from '@/types/Peoples'

export default defineComponent({
  components: { Spinner },
  setup() {

    const loading = ref(true as boolean)
    const peoples = ref({} as Peoples)

    onMounted( () => getPeople())

    const getPeople = () => {
      DataService.getAll()
        .then((res: ResponseData) => {
          peoples.value = res.data
          loading.value = false
        })
        .catch((e: Error) => console.log(e))
    }

    return {
      loading,
      peoples
    }

  }
})
</script>
