<template>
  <form action='/' @submit.prevent='onSubmit'>
    <input v-model='positionInputModel' type='text' name='name' placeholder='Должность'>
    <br>
    <HomeFormPersonalFields v-model='personalDataModel' />
    <br>
    <button type='submit'>Отправить</button>
  </form>
</template>

<script lang='ts'>
export default {
  name: 'HomeForm'
}
</script>

<script setup lang='ts'>
import { ref } from 'vue'
import HomeFormPersonalFields from '@/components/HomeFormPersonalFields.vue'

const positionInputModel = ref('')
const personalDataModel = ref<{ name: string, lastName: string }>({ name: '', lastName: '' })

const onSubmit = () => {
  alert('Должность - ' + positionInputModel.value + ', Имя - ' + personalDataModel.value.name + ', Фамилия - ' + personalDataModel.value.lastName)
}
</script>