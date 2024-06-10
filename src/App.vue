<template>
    <div class="header">
      <img src="./assets/Sims4Logo.png" alt="">
      <h1>Postcards Collection Checklist</h1>
    </div>
    <Postcards v-if="postcardsHeader.length && postcardList.length" 
                 :postcardsHeader="postcardsHeader" 
                 :postcardList="postcardList" />
</template>
  
<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';
import Postcards from './components/Postcards.vue';

interface PostcardsData {
    header: string[];
    postcards: string[];
}

export default defineComponent({
name: 'App',
components: {
    Postcards
},
setup() {
    const postcardsHeader = ref<string[]>([]);
    const postcardList = ref<string[]>([]);

    onMounted(async () => {
        const response = await fetch('/postcards-list.json');
        const data: PostcardsData[] = await response.json();
        if (data.length > 0) {
            postcardsHeader.value = data[0].header;
            postcardList.value = data[0].postcards;
        }
    });

    return {
        postcardsHeader,
        postcardList
    };
}
});
</script>

<style>
body {
    background-color: #415593;
}

#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #415593;
    background-color: white;
    margin: auto;
    margin-top: 60px;
    border-radius: 15px;
    padding: 20px 10px 10px;
    box-sizing: border-box;
    width: fit-content;
    display: flex;
    flex-direction: column;
    align-content: center;
    text-align: center;
}

.header {
    padding-bottom: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.header * {
    display: inline;
}

.header img {
    max-height: 40px;
    margin-right: 15px;
}
</style>
