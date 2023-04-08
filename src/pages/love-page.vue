<template>
  <q-dialog v-model="editor" persistent>
    <q-card flat bordered>
      <q-btn @click="close" flat style="position: absolute; right: 0; top: 6px; z-index: 100; color: #aaaaaa" icon="highlight_off" size="20px"></q-btn>
      <q-img alt="" src="photo.jpg"></q-img>
      <div style="text-align: center; font-size: 25px; margin-top: 20px">Надзвичайно сильно<br> кохаю тебе, моя Нікушка</div>
      <div style="text-align: center; font-size: 20px; margin-top: 10px">Завжди пам'ятай це</div>
      <q-icon name="favorite" size="50px" style="margin: 15px auto;"></q-icon>
    </q-card>

  </q-dialog>
</template>

<script setup>
import {ref, watch} from "vue"
const editor = ref(true)
const emit = defineEmits(['on-cancel'])

function close() {
  emit('on-cancel')
}

</script>
