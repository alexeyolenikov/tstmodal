<template>
  <teleport to='body'>
    <div v-if='isOpen' class='ui-popup' @click.self='close'>
      <div class='ui-popup-inner'>
        <slot :close='close' :confirm='confirm'>
          <button @click='close'>Close</button>
          <button @click='confirm'>Confirm</button>
        </slot>
      </div>
    </div>
  </teleport>
</template>

<script lang='ts'>
export default {
  name: 'UIPopup'
}
</script>

<script setup lang='ts'>
import { onBeforeUnmount, ref } from 'vue'

const isOpen = ref(false)
const actionCallbacks = {} as {
  resolve: (v: unknown) => void
  reject: (v: unknown) => void
}

const show = () => {
  isOpen.value = true
  document.addEventListener('keyup', keyUpHandler)

  return new Promise((resolve, reject) => {
    actionCallbacks.resolve = resolve
    actionCallbacks.reject = reject
  })
}

const close = () => {
  actionCallbacks.reject(false)
  isOpen.value = false
  document.removeEventListener('keyup', keyUpHandler)
}

const confirm = () => {
  actionCallbacks.resolve(true)
  close()
}

const keyUpHandler = (e: KeyboardEvent) => {
  if (e.key === 'Escape') {
    close()
  }
}

onBeforeUnmount(() => {
  document.removeEventListener('keyup', keyUpHandler)
})

defineExpose({ show })

</script>

<style scoped>
.ui-popup {
  margin-top: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0, .4);
}

.ui-popup-inner {
  padding: 30px;
  width: 300px;
  background-color: #fff;
}
</style>
