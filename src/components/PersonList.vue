<template>
    <div>
        <div class="list" v-for="person in people">
            <personWithSave :personInfo="person" :existsInDb="true" v-on:update-list="updateList"></personWithSave>
        </div>
        <div class="buttonGroup">
            <button @click="newPerson">Add New</button>            
        </div>
    </div>
</template>

<script>
// Imports
import personWithSave from './PersonWithSave.vue';

export default {
    components: {
        personWithSave
    },

    data () {
        return {
            people: []
        }
    },

    methods: {      
        getPeople: function(){  
            var getAllUrl = 'http://localhost:8080/api/person/getall'; 

            this.$http.get(getAllUrl).then(response => {

            // get body data
            this.people = response.body;

            }, response => {
            // error callback
            });
        },

         newPerson: function(body){
            var newPerson = {
                "name" : "Newperson", 
                "age" : "0"
                };
                var createUrl = 'http://localhost:8080/api/person/create';
            this.$http.post(createUrl, newPerson).then(response => {

                // show response status
                console.log(response.status);
                // update persons with list sent from backend
                this.people = response.body;

                }, response => {
                    // error callback
            });         
        },

         updatePerson: function(body){
             var updateUrl = 'http://localhost:8080/api/person/update';

            this.$http.put(updateUrl, body).then(response => {

                // show response status
                console.log(response.status);
                // update persons with list sent from backend
                this.people.push(response.body);

                }, response => {
                    // error callback
            });
        },

        updateList: function(data){
           this.people = data;
        }
    },

    created() {
        this.getPeople();
    }
}
</script>

<style>
.list {
    display: flex;
    align-items: center;
    justify-content: center;
    padding-top: 20px;
}

.buttonGroup {
    padding-top: 30px;
}

</style>