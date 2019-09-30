<template>
    <div>
        <div v-show = "deleted == false">
            <div class="inputForm">
                <div v-show = "edited == false">
                    <label class="personElement"> Person: {{ personInfo.name }} </label>
                </div>

                <input v-show = "edited == true" v-model = "personInfo.name">

                <div v-show = "edited == false">
                    <label class="personElement"> Age: {{ personInfo.age }} </label>
                </div>
                <div class="buttonCollection">
                    <input v-show = "edited == true" v-model = "personInfo.age">
                    <button v-show = "edited == false" @click = "setEdited">Edit</button>
                    <button v-show = "edited == true" @click = "undo">undo</button>
                    <button v-show = "edited == true" @click = "runValidation">Accept</button>
                    <button v-show = "edited == false" @click = "remove">Delete</button>
                </div>
            </div>
            <div class="validation" v-show = "validationFailed == true">
                <p>Name can't be empty, Age must be 1-99</p> 
            </div>
        </div>
        <div class="deletedItem" v-show = "deleted == true">
            <label class="personElement"> DELETED - Person: {{ personInfo.name }} </label>
            <label class="personElement"> Age: {{ personInfo.age }} </label>
        </div>
    </div>
</template>
  
<script>
export default {
    data () {
        return {
            edited: false,
            validationFailed: false,
            deleted: false,
            tempPersonInfo: this.personInfo
            
        }
    },
    props: [
        'personInfo'
    ], 
    methods: {
        setEdited() {
            this.tempPersonInfo = this.personInfo;
            this.edited = true;
        },

        undo: function() {
            console.log(this.tempPersonInfo.age);
            this.personInfo = this.tempPersonInfo;       
            this.edited = false;
            this.$emit('ok-to-send', true);
        },

        accept: function() {
            this.edited = false;
        },

         runValidation: function() { //Validates Name and Age inputs.
            if(this.personInfo.age > 0 && 
            this.personInfo.age < 100 &&
            this.personInfo.name != ""){
                this.edited = false;
                this.$emit('ok-to-send', true);
                this.accept();
            }
            else{
                this.$emit('ok-to-send', false);
                console.log("Validation failed!");
            }
        },

         remove: function() {
            this.deleted = true;
            this.$emit('remove-person', this.personInfo);
        }
    }
  

}

</script>

<style>
    .inputForm {
    display: flex; 
    margin: 0 auto;  
    }

    .personElement {
        padding-left: 10px;
    }

    .buttonCollection {
        padding-left: 20px;
    }

    .validation {
        font-family: "Comic Sans";
        color: red;
    }

    .deletedItem {
        background-color:red;
    }

</style>