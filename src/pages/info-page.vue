<template>
  <q-dialog v-model="editor" persistent>
  <q-card flat bordered>
    <q-btn @click="close" flat style="position: absolute; right: 0; top: 6px; z-index: 100; color: #aaaaaa" icon="highlight_off" size="20px"></q-btn>

    <div style="margin: 0 auto; width: 100%; height: 100%">
    <div v-for="shift in props.shifts" :key="shift"
         :style="'width: 100%; height: 33.3%; display: flex; ' +
          'align-items: center; justify-content: center; background:' + shift.color">
        <div style="text-align: center; font-size: 30px; color: #85858C">{{ shift.text }}</div>
    </div>
    </div>
  </q-card>

  </q-dialog>
</template>

<script setup>
import {ref, watch} from "vue"
const editor = ref(true)
const emit = defineEmits(['on-cancel'])

const props = defineProps({
  shifts: Object
})

function close() {
  emit('on-cancel')
}

</script>
