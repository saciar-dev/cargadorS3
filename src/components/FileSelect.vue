<template>
  <div class="q-pa-md" style="max-width: 300px">
    <div class="q-gutter-md">
      <q-file v-model="model" label="Abrir archivo" />
    </div>
    <div v-if="isloaded" >
      <div v-for="url in imagesURL" class="flex ">
        <q-img
          :src="url.url"
          spinner-color="white"
          style="height: 259px; max-width: 150px"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
  import axios from 'axios';
import {api} from 'src/boot/axios';
  import { ref, watch, onMounted } from 'vue';

  const model = ref(null);
  const club = ref({});
  const isloaded = ref(false);
  const imagesByClub = ref([]);
  const imagesURL = ref([]);

  //const url = ref("http://localhost:8080/image/1/download");

  onMounted(async()=>{
    const response = await api.get("/club/1");
    club.value = response.data;
    if(club.value.id){
      loadImages();

    }

  })

  const loadImages = async() =>{
    isloaded.value = true;
    const response = await api.get("/image/club/1");
    imagesByClub.value = response.data;
    imagesByClub.value.map( image =>{
      imagesURL.value.push({url:`http://localhost:8080/image/${club.value.id}/download/${image.id}`})
    })
    console.log(imagesURL.value);
  }

  watch(model, async() => {
    console.log(model.value);
    const formData = new FormData();
    formData.append("file", model.value);
    await api.post(`/image/${club.value.id}/upload`, formData, {headers: {
      "Content-Type": "multipart/form-data"
    }});
  });

</script>
