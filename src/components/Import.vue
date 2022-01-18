<template>
    <v-container>
        <v-row align="center">
            <v-col>
                <v-card 
                class="mx-auto my-12"
                max-width="750"
                height="300">
                    <v-card-title class="d-flex justify-center text-h3">Import</v-card-title>
                    <v-row class="ma-0">
                        <v-col cols="9">
                            <v-file-input
                            id="inputFile"
                            label="Search file"
                            counter
                            truncate-length="15"
                            solo
                            type="file"
                            v-model="resetFile"
                            @change="fileValue">
                            </v-file-input>
                        </v-col>
                        <v-col cols="2">
                            <v-btn
                            color="blue"
                            class="ma-2 white--text"
                            @click="addRecord">
                            Upload
                            <v-icon
                                right
                                dark>
                            mdi-cloud-upload
                            </v-icon>
                            </v-btn>
                        </v-col>
                    </v-row>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>
<script>
import XLSX from "xlsx";
export default {
    computed:{
        
      alldata:{
          get(){
            return this.$store.state.alldata;
          },
          set(data){
            this.$store.commit('setAlldata', data);
          }
      },
    },
    data(){
        return{
            resetFile:{},
        }
    },
    methods:{
                     fileValue(vueVal){
            if(vueVal===null){
                //console.log("Null"); 
            }else{
                let that=this;
                //console.log(vueVal); 
                var reader = new FileReader();
                reader.onload = function(vueVal) {

                    var data = new Uint8Array(vueVal.target.result);
                    var workbook = XLSX.read(data, {type: 'array'});
                    let sheetNames = workbook.SheetNames;
                    // /* DO SOMETHING WITH workbook HERE */
                    that.dataFile=workbook;
                    ////console.log(that.dataFile); 
                    that.sheets=sheetNames[0];
                    let worksheet = workbook.Sheets[that.sheets];
                    that.files=XLSX.utils.sheet_to_json(worksheet);
                    //console.log(XLSX.utils.sheet_to_json(worksheet));
                    
                    
                };
                reader.readAsArrayBuffer(vueVal);
            }
        },
        addRecord(){
          let arrayData=this.alldata;
            //console.log(this.files);
          
          this.files.forEach(element => {
                arrayData.push(element);
            });
          this.$store.commit('setAlldata', arrayData);
            this.resetFile={};

        },
    }
}
</script>