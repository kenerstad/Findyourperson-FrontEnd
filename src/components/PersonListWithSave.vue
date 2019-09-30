<template>
    <div>
        <div class="listWSave" v-for="person in people">
            <person :personInfo="person" :existsInDb="true" 
            v-on:ok-to-send="checkOkToSend" 
            v-on:remove-person="removePerson">
            </person>
        </div>
        <div class="buttonGroup">
            <button @click="newPerson">Add New</button>
            <button @click="save">Save</button>
        </div>
    </div>
</template>

<script>
// Imports
import person from './Person.vue';

export default {
    components: {
        person,
    },

    data () {
        return {
            people: [],
            okToSend: true,
            idCounter: -1
        }
    },

    methods: {      
        getPeople: function(){  
            var getAllUrl = 'http://localhost:8080/api/person/getall'; 

            this.$http.get(getAllUrl).then(response => {
            
            // show response status
            console.log(response.status);
            // update persons with list sent from backend
            this.people = response.body;

            }, response => {
            // error callback
            });
        },

        checkOkToSend(data){
            // Set ok/not ok to send data to back-end after validation
            this.okToSend = data;
        },

        /*
        * Creates new person in frontend only, sets id to a negative number
        * in order for frontend to keep track of persons not committed to back-end. 
        */
         newPerson: function(body){ 
         
             this.people.push({"id" : this.idCounter, "name" : "new", "age" : "0"});
             this.idCounter--;
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

        removePerson: function(data){
            console.log(data.id);
            if(data.id < 0){
                var peeps = this.people;
                peeps.forEach(function (item, index) {
                    if(item.id == data.id){
                        peeps.splice(index, 1);
                    }                  
                });
                this.people = peeps;
            }
            else{
                var peeps = this.people;
                peeps.forEach(function (item, index) {
                    if(item.id == data.id){
                        item.name = "DELETED"
                        peeps[index] = item;
                    }                  
                });
            }
        },

        save: function(){
            if(!this.okToSend){
                return;
            }
            var saveUrl = 'http://localhost:8080/api/person/save';

            this.$http.post(saveUrl, this.people).then(response => {
                // show response status
                console.log(response.status);
                // update persons with list sent from backend
                this.people = response.body;

                }, response => {
                    // error callback
            });
        }
    },

    created() {
        this.getPeople();
    }
}
</script>

<style>
.listWSave {
    display: flex;
    align-items: center;
    justify-content: center;
    padding-top: 20px;
}

.buttonGroup {
    padding-top: 30px;
}

</style>