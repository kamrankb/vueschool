<template>
    <section v-if="destination.destination" class="destination">
        <h1>{{ destination.destination.name }}</h1>
        <div class="destination.destination-details">
            <img :src="`/images/${destination.destination.image}`" :alt="destination.destination.name">
            <p>{{ destination.destination.description }}</p>
        </div>
    </section>

    <section v-if="destination.destination" class="experiences">
        <h2>Top Experiences in {{ destination.destination.name }}</h2>

        <div class="cards">
            <ExperienceCard 
                v-for="experience in destination.destination.experiences"
                :key="experience.slug"
                :experience="experience"
            />
        </div>
    </section>
</template>

<script setup>
import ExperienceCard from '@/components/ExperienceCard.vue';
import { computed, reactive } from 'vue';
import { useRoute } from 'vue-router'
const route = useRoute();

const destination = reactive({destination: null});
const destinationId = computed(() => {
    return parseInt(route.params.id);
});

async function initData() {
    const response = await fetch(`https://travel-dummy-api.netlify.app/${route.params.slug}`);
    destination.destination = await response.json();    
    console.log(destination.destination);
}

initData();
//this.$watch(() => this.$route.params, this.initData);

</script>