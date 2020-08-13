<template>
    <v-app>
        <v-app-bar app dark>
            <v-toolbar-title><v-icon>mdi-airplane</v-icon>Airport</v-toolbar-title>
        </v-app-bar><br><br><br>
        <v-container>
            <v-card>
                <v-card-text>
                    <v-layout>
                        <v-flex lg3 md3 sm6 xs12>
                            <v-select label="เลือกประเภทสนามบิน:" :items="airportInfo" v-model="sortTypeAir"
                            @change="sortTypeAirport" item-text="name" item-value="id"></v-select>
                        </v-flex>
                    </v-layout><br>
                    <v-data-table :headers="headAirport" :items="Info" class="elevation-3" dense>
                        <template v-slot:no-data>
                            <v-progress-linear v-if="progressData"  height="10px" 
                            v-slot:progress color="primary" indeterminate></v-progress-linear>
                            <v-flex v-if="noData">
                                <strong><font color="red">ไม่พบข้อมูล</font></strong>
                            </v-flex>
                        </template>
                        <template v-slot:item.btn="{ item }">
                            <v-btn outlined color="primary" small @click="getDetail(item)">ดูรายละเอียด</v-btn>
                        </template>
                    </v-data-table>

                    <v-dialog v-model="showDetail" max-width="500" persistent>
                        <v-card>
                            <v-card-title>รายละเอียด</v-card-title>
                            <v-divider></v-divider>
                            <v-card-text>
                                <br>
                                <v-layout>
                                    <v-flex xs6 text-right pr-5>ชื่อสนามบิน (ภาษาไทย):</v-flex>
                                    <v-flex>{{nameTh}}</v-flex>
                                </v-layout>
                                <v-layout>
                                    <v-flex xs6 text-right pr-5>ชื่อสนามบิน (ภาษาอังกฤษ):</v-flex>
                                    <v-flex>{{nameEng}}</v-flex>
                                </v-layout>
                                <v-layout>
                                    <v-flex xs6 text-right pr-5>ประเภทสนามบิน:</v-flex>
                                    <v-flex>{{typeAir}}</v-flex>
                                </v-layout>
                                <v-layout>
                                    <v-flex xs6 text-right pr-5>จังหวัด:</v-flex>
                                    <v-flex>{{provin}}</v-flex>
                                </v-layout>
                                <v-layout>
                                    <v-flex xs6 text-right pr-5>รหัสท่าอากาศยาน:</v-flex>
                                    <v-flex>{{idAir}}</v-flex>
                                </v-layout>
                                <v-layout>
                                    <v-flex text-right><v-btn small color="primary" @click="showDetail = false">Close</v-btn></v-flex>
                                </v-layout>
                            </v-card-text>
                        </v-card>
                    </v-dialog>
                </v-card-text>
            </v-card>
        </v-container>
    </v-app>
</template>
<script>
import axios from 'axios'
export default {
    data(){
        return{
            progressData: false,
            noData: true,
            nameTh: '',
            nameEng: '',
            typeAir: '',
            provin: '',
            idAir: '',
            showDetail: false,
            Info: [],
            infoClone: [],
            headAirport: [
                { text: 'ลำดับ', align: 'left', value: 'index' },
                { text: 'ชื่อสนามบิน', align: 'left', value: 'name' },
                { text: 'ประเภทสนามบิน', align: 'left', value: 'typeName' },
                { text: 'รายละเอียด', sortable: false, align: 'center', value: 'btn' },
            ],
            sortTypeAir: '',
            airportInfo: [],
        }
    },
    methods:{
        getDetail(item){
            this.nameTh = item.nameTh
            this.nameEng = item.nameEn
            this.typeAir = item.typeName
            this.provin = item.city
            this.idAir = item.icao
            this.showDetail = true
        },

        sortTypeAirport(){
            // this.Info = []
            // this.noData = true
            // this.progressData = true
            this.Info = this.infoClone.filter(e=>e.typeId == this.sortTypeAir)
            if(this.Info.length == 0){
                this.Info = this.infoClone
            }
        }
    },
    mounted(){
        axios.get("https://my-json-server.typicode.com/salakjitp/airport-api/airportType",{})
        .then(res=>{
            let result = res.data.list
            this.airportInfo = result
        })
        this.noData = false
        this.progressData = true
        axios.get("https://my-json-server.typicode.com/salakjitp/airport-api/airports",{})
        .then(res=>{
            let result = res.data
            let i = 1
            result.forEach(el=>{
                this.infoClone.push({ index: i, name: el.nameTh + " (" + el.nameEn + ")", ...el })
                this.Info.push({ index: i, name: el.nameTh + " (" + el.nameEn + ")", ...el })
                i++
            })
            
        })
    }
}
</script>