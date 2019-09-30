<template>
    <div>
        <div class="flex">
            <div v-show = "edited == false">
                <label class="personElement"> Person: {{ personInfo.name }} </label>
            </div>

            <input v-show = "edited == true" v-model = "personInfo.name">

            <div v-show = "edited == false">
                <label class="personElement"> Age: {{ personInfo.age }} </label>
            </div>
            <div class="buttonCollection">
                <input v-show = "edited == true" v-model = "personInfo.age">
                <button v-show = "edited == false" @click = "edited = true">Edit</button>
                <button v-show = "edited == true" @click = "undo">undo</button>
                <button v-show = "edited == true" @click = "runValidation">Accept</button>
                <button v-show = "edited == false" @click = "remove">Delete</button>
            </div>
        </div>
        <div class="validation" v-show = "validationFailed == true">
            <p>Name can't be empty, Age must be 1-99</p> 
        </div>
      </div>
</template>
  
       

<script>
export default {
    data () {
        return {
            edited: false,
            tempName: this.personInfo.name,
            tempAge: this.personInfo.age,
            validationFailed: false
        }
    },
    props: [
        'personInfo'
    ], 
    methods: {

        undo: function() {
            this.personInfo.name = this.tempName;
            this.personInfo.age = this.tempAge;
            this.edited = false;
        },

        accept: function() {                  
            this.$http.put('http://localhost:8080/api/person/update', this.personInfo).then(response => {

            // get body data
            this.people = response.body;
            console.log(response.body);
            console.log(response.status);
            }, response => {
            // error callback
            });
        },

         runValidation: function() {
            if(this.personInfo.age >= 0 && 
            this.personInfo.age < 100 &&
            this.personInfo.name != ""){
                this.edited = false;
                this.validationFailed = false;
                this.accept();
            }
            else{
                this.validationFailed = true;
                console.log("Validation failed!");
            }
        },

         remove: function() {
            this.$http.post('http://localhost:8080/api/person/delete', this.personInfo).then(response => {

       
            this.$emit('update-list', response.body);
            console.log(response.body);
            console.log(response.status);
            }, response => {
            // error callback
            });
        }
    },
    

}

</script>

<style>
    .flex {
    display: flex; 
    margin: 0 auto;  
    }

    .personElement {
        padding-left: 10px;
    }

    .buttonCollection {
        padding-left: 20px;
    }

    .deleteButton {
        padding-left: 20px;
    }

    .validation {
        font-family: "Comic Sans";
        color: red;
    }

</style>