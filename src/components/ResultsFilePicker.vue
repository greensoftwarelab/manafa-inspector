<template>
  <div class="ma-4 filepicker">
    <h3 class="ma-2">Results File: {{loaded_file}}</h3>
    <v-file-input
        accept="application/json"
        label="click to select file"
        v-if="!loaded"
        class="ma-2"
        v-model="my_file"
        show-size>
    </v-file-input>
    <v-btn class="ma-2" @click="load_unload()">{{btn_text}}</v-btn>

  </div>
</template>

<script>
export default {
    data(){
        return {
            my_file: null,
            loaded: false,
            loaded_file: ''
        }
    },
    methods: {
        load_unload(){
            if (this.my_file != null && !this.loaded){
                this.loaded = true
                this.loaded_file = this.name()
                this.$emit('filechange', this.my_file)
            }
            else if (this.loaded){
                this.loaded_file=''
                this.loaded = false
            }
        },
        name(){
            return this.my_file != null ? this.my_file.name : ''
        },
    },
    computed:{
        btn_text(){
            return this.loaded ? "X" : "Load"
        }
    }
}
</script>

<style>

.filepicker{
    display: flex;
    flex-direction: row;
    justify-content: center;
    text-align: center;
    align-content: center;
    align-items: center;
}

</style>