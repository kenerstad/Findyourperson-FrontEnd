<template>
    <div>
        <h1>People!</h1>
        <h3> This list of people updates on the fly!</h3>   
        <person-list></person-list>
    </div>
</template>

<script>
// Imports
import personList from '../components/PersonList.vue';

export default {
    components: {
        'person-list': personList
    },
    data () {
        return {
            
        }
    },
    methods: {
   
    }
}
</script>