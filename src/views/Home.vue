<template>
  <main>
    <TimeTracker />
  </main>
</template>

<script>
import TimeTracker from '@/components/TimeTracker';
import { ref } from 'vue';

export default {
  name: 'Home',
  components: {
    TimeTracker
  },
  setup () {
    const fetchUserData = async () => {
      //Llamada axios a la API de Sesame que devuelve un error CORS.
      const res = await axios.request({
            url: '/',
            method: 'get',
            baseURL: 'https://api.sesametime.com/schedule/v1/work-entries',
            headers: {
                'Content-Type' : 'application/json;charset=utf-8',
                'Access-Control-Allow-Origin' : '*',
                'Authorization': 'Bearer 9a4c685e6fca0347ca0775119e9c47b6ba00e49096ba99ffcf6a41fe06e6f001'
                
            }
          });
          return await res.json();  
    }

    const baseSetup = async () => {
      //const data = await fetchUserData();
      const data = '../assets/response.json'
    };

    baseSetup();

  }
};
</script>
