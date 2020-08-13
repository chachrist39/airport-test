<template>
    <v-app>
        <mainMenu></mainMenu>
        <v-container>
            <v-card>
                <v-card-text></v-card-text>
            </v-card>
        </v-container>
    </v-app>
</template>
<script>
import mainMenu from '@/components/menu'
// import axios from 'axios'
export default {
    components:{
        mainMenu
    },
    data(){
        return{

        }
    },
}
</script>