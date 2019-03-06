<template>
    <div class="main-content">
        <div class="nes-textarea textarea-color">
            <h1> Challenge Journey</h1>
            <h2> How many brave souls participate? </h2>

            <div v-for="(item, index) in participants" :key="index" v-on:input="checkEmptyField">

                <input v-if="index == inputFields" type="text" name="participants" class="input nes-input" v-bind:id="`${index}`"
                    v-on:input="newPlayer"  v-on:keyup.13="checkForEnter" v-model="item.name" placeholder="Enter name...">
                <input v-else type="text" name="participants" class="input nes-input" v-bind:id="`Else-${index}`" v-on:keyup.13="checkForEnter"
                    v-model="item.name">

            </div>

            <button type="button" class="nes-btn is-error" @click="resetForm">Reset</button>
            <button type="button" class="nes-btn is-primary" @click="processForm">Challenge accepted</button>
            
            <p>{{ alert }}</p>
        </div>
    </div>
</template>

<script>
    export default {
        name: "main-content",
        data: () => ({
            //keeps track of the number of names currently entered
            editIndex: null,
            //represents the number of created input fields
            inputFields: 0,

            participants: [
                { name: ''  }
            ],

            alert: ""
        }),

        methods: {
            processForm: function() {
                if(this.participants.length === 1) {
                    this.alert = "Please enter at least one name."
                }

                let cleanedParticipants = [];
                let cleanedNames = "";

                for(let n in this.participants) {
                    cleanedNames = this.participants[n].name;
                    cleanedNames = cleanedNames.trim();

                    if(cleanedNames !== "") {
                        cleanedParticipants.push({ name: cleanedNames })
                    }
                  //  console.log(`Index: ${n}, Names: ${this.participants[n].name}`);
                }
     
                let objJSON = JSON.stringify(cleanedParticipants);

                //print data to json OBJ 
                console.log(objJSON);
                
            },

            resetForm: function() {
                this.participants = [ { name: ''  } ];
                this.inputFields = 0;
                this.editIndex = null;
                this.alert = "";
            },

            newPlayer: function() { 
                this.editIndex = this.participants.length - 1        
                let inputId = document.getElementsByClassName("input")[this.inputFields];

                //third attribute of inputId is: v-bind:id="`${index}`"
                let index = inputId.attributes[3].value;      

                //check that spaces doesn't create new inputs
                if(index == this.editIndex && inputId.value.trim() !== "") {
                    this.participants.push({ name: '' })
                    this.inputFields += 1
                }
                
            },

            checkForEnter: function() {
                let currentFocus = document.activeElement.id; 
                currentFocus = currentFocus.replace("Else-", "");
                currentFocus = parseInt(currentFocus);
                let trackId = (`Else-${currentFocus+1}`);
                let inputValue = document.getElementsByClassName("input")[currentFocus].value;
        
                if (inputValue.trim() !== "") {
                    if(this.inputFields === (currentFocus + 1) && (currentFocus <= this.editIndex)) {
                        document.getElementById(currentFocus + 1).focus();
                    } else if ((currentFocus <= this.editIndex)){
                        document.getElementById(trackId).focus();
                    }
                }
                
                    
                
            },

            // remove input field if it is empty
            checkEmptyField: function () {
                
                //second to last input only exists if there is more than one input field
                if (this.inputFields > 0) {
                    let secondToLastInput = document.getElementsByClassName("input")[this.inputFields - 1].value;

                    //check if input field is empty
                    if (secondToLastInput === "") {
                        this.participants.splice(this.inputFields - 1, 1);
                        this.inputFields -= 1;
                    }

                    for (let i = 0; i < (this.inputFields); i++) {
                        let checkAllInputs = document.getElementsByClassName("input")[i].value;

                        //if input field is empty, remove it
                        if (checkAllInputs === "" ) {
                            this.participants.splice(i, 1);
                            this.inputFields -= 1;
                        }

                    }

                }

            }

            
        }
    

    }

</script>

<style scoped>
    .main-content {
        margin: 0 auto;
        font-family: 'Press Start 2P', cursive;
        padding: 10px;
        max-width: 600px;
    }

    .textarea-color {
        background-color: #86cfd0;
    }


    .input {
        margin-bottom: 20px;
    }


</style>