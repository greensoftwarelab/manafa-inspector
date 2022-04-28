<template>
    <v-card class="ma-4 main_card">
        <v-list-group :value="true" class="large_width"> 
            <template v-slot:activator>
                <v-icon size="30" >mdi-text-box-multiple-outline</v-icon>
                <v-list-item-title><h2>Methods</h2></v-list-item-title>
            </template>
            <v-text-field
                v-model="search"
                class="ma-4"
                append-icon="mdi-magnify"
                label="Search"
                single-line
                hide-details
            >
            </v-text-field>
                <v-data-table
                :headers="headers()"
                :items="tablevalues()"
                :search="search"
                >
            </v-data-table>
            <!--v-list-group v-for="(x,i) in Object.keys(stuff)" :key="i" link :value="true"> 
                <template v-slot:activator>
                    <v-icon size="20" >mdi-file-chart-check</v-icon>
                    <v-list-item-title><h4>{{x}}</h4></v-list-item-title>
                </template>
                <method-card :method="x" :calls_of_method="stuff[x]" ></method-card>
            </v-list-group-->
        </v-list-group>

    </v-card>

</template>

<script>

//import MethodCard from './MethodCard.vue'

export default {
    name: 'MethodsCard',
    props: {
        called_methods: {
        type: Object,
        default: () => {
            return {
                }
            }
        }
    },
    data(){
        return {
            search:'',
        }
    },
    methods: {
        headers(){
            const the_l = [ { text: 'mid', value: 'mid' },{ text: 'call', value: 'call' }]
            this.getHeaders().forEach(i => {
                the_l.push( { text: i, value: i} )
            });
            return the_l
        },
        getHeaders(){
            if (Object.keys(this.called_methods).length == 0){
                return []
            }
            //console.log(Object.keys(Object.values(Object.values(this.called_methods)[0])[0]))
            return Object.keys(Object.values(Object.values(this.called_methods)[0])[0])
        },
        tablevalues(){
            //console.log(Object.keys(this.called_methods)[0])
            const the_l = []
            Object.keys(this.called_methods).forEach(i => {
                Object.keys(this.called_methods[i]).forEach( e =>{
                    the_l.push(
                        {
                            mid : i,
                            call: e,
                            ...this.called_methods[i][e]
                        }
                    )
                })
            });
            return the_l
        },
        getTableValues(){
            return []
        },
        isDict(v) {
            return typeof v==='object' && v!==null && !(v instanceof Array) && !(v instanceof Date);
        }
    },
    created(){
        this.getTableValues()
    }
}
</script>

<style>


</style>