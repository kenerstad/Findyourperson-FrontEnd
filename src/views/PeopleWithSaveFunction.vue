<template>
    <div>
        <h1>People with save!</h1>
        <h3> Edit the list of people in frontend then update to backend all in one go when save button is pressed.</h3> 
        <person-list></person-list>
    </div>
</template>

<script>
// Imports
import personList from '../components/PersonListWithSave.vue';

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
