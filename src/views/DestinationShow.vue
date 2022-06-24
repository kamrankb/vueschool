<template>
    <section v-if="destination" class="destination">
        <h1>{{ destination.name }}</h1>
        <div class="destination-details">
            <img :src="`/images/${destination.image}`" :alt="destination.name">
            <p>{{ destination.description }}</p>
        </div>
    </section>

    <section v-if="destination" class="experiences">
        <h2>Top Experiences in {{ destination.name }}</h2>

        <div class="cards">
            <ExperienceCard 
                v-for="experience in destination.experiences"
                :key="experience.slug"
                :experience="experience"
            />
        </div>
    </section>
</template>

<script>
import ExperienceCard from '@/components/ExperienceCard.vue';

export default {
    components: {ExperienceCard},

    data() {
        return {
            destination: null
        }
    },

    computed:{
        destinationId(){
            return parseInt(this.$route.params.id);
        },
    },

    methods: {
        async initData() {
            const response = await fetch(`https://travel-dummy-api.netlify.app/${this.$route.params.slug}`);
            this.destination = await response.json();    
        }
    },  

    async created() {
        
        this.initData();

        //this.$watch(() => this.$route.params, this.initData);
    }
}
</script>