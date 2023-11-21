<template>
  <q-page class="flex flex-center">
    <FileSelect></FileSelect>
  </q-page>
</template>

<script>
import FileSelect from 'src/components/FileSelect.vue';
import { defineComponent } from 'vue'

export default defineComponent({
    name: 'IndexPage',
    components: { FileSelect }
})
</script>
