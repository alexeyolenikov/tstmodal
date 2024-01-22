<template>
  <button type='button' @click.prevent='onClick'>Ввести персональные данные</button>
  <UIPopup ref='popupRef'>
    <template #default='{ close, confirm }'>
      <input v-model='nameInputModel' type='text' name='name' placeholder='Имя'>
      <input v-model='lastNameInputModel' type='text' name='name' placeholder='Фамилия'>
      <br><br>
      <button type='button' @click.prevent='confirm'>Сохранить</button>
      <button type='button' @click.prevent='close'>Отменить</button>
    </template>
  </UIPopup>
</template>

<script lang='ts'>
export default {
  name: 'HomeFormPersonalFields'
}
</script>

<script setup lang='ts'>
import UIPopup from './UIPopup.vue'
import { ref } from 'vue'

const emit = defineEmits([ 'update:modelValue' ])
defineProps<{ modelValue: { name: string, lastName: string } }>()

const popupRef = ref<typeof UIPopup>()
const lastNameInputModel = ref('')
const nameInputModel = ref('')

const onClick = () => {
  popupRef.value?.show()
    .then(() => {
      emit('update:modelValue', {
        name: nameInputModel.value,
        lastName: lastNameInputModel.value
      })
    })
}
</script>