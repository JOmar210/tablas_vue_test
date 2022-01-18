<template >
  <v-app class="ma-5">
    <div class="main">
      <v-container class="ma-3">
        <v-row >
          <v-col class="d-flex justify-end pa-0" >
            <v-btn
              color="blue"
              class="mb-4 white--text"
              @click="downloadAsExcel">
              Download
              <v-icon
                right
                dark>
              mdi-cloud-download
              </v-icon>
            </v-btn>
          </v-col>
        </v-row>
        <v-row>
          <v-col class="pa-0">    
            <v-data-table
              :headers="head"
              :items="tableData"
              sort-by="calories"
              class="elevation-1"
              :search="search"
              :custom-filter="filterOnlyCapsText">
                <template v-slot:top>
                  <v-toolbar
                  flat>
                    <!-- <v-toolbar-title>My CRUD</v-toolbar-title> -->
                    <v-text-field
                      v-model="search"
                      label="Search"
                      class="mx-4">
                    </v-text-field>
                    <v-spacer></v-spacer>
                    <Popup/>
                    <v-btn
                      color="primary"
                      dark
                      class="mb-2 mr-2"
                      @click="filter(2)">
                      Tier 1
                    </v-btn>
                    <v-btn
                      color="primary"
                      dark
                      class="mb-2 mr-2"
                      @click="filter(3)">
                      Tier 2
                    </v-btn>
                    <v-btn
                      color="primary"
                      dark
                      class="mb-2 mr-2"
                      @click="filter(4)">
                      Tier 3
                    </v-btn>
                    <v-btn
                      color="primary"
                      dark
                      class="mb-2 mr-2"
                      @click="filter(5)">
                      SL
                    </v-btn>
                    <v-btn
                      color="primary"
                      dark
                      class="mb-2 mr-2"
                      @click="filter(1)">
                      Reset
                    </v-btn>
                  </v-toolbar>
                </template>
            </v-data-table>
          </v-col>
        </v-row>
      </v-container>
    </div>
  </v-app>
</template>
<script>
import XLSX from "xlsx";
import Popup from "./PopupColumns.vue"
import filesaver from '../filesaver'
  export default {
    components:{Popup},
    computed: {
      columns:{
          get(){
            return this.$store.state.columns;
          },
          set(data){
            this.$store.commit('setColumns', data);
          }
      },
      customHeaders:{
          get(){
            return this.$store.state.customHeaders;
          },
          set(data){
            this.$store.commit('setCustomHeaders', data);
          }
      },
      headers:{
          get(){
            return this.$store.state.headers;
          },
          set(data){
            this.$store.commit('setHeaders', data);
          }
      },
      headersF1:{
          get(){
            return this.$store.state.headersF1;
          },
          set(data){
            this.$store.commit('setHeadersF1', data);
          }
      },
      headersF2:{
          get(){
            return this.$store.state.headersF2;
          },
          set(data){
            this.$store.commit('setHeadersF2', data);
          }
      },
      headersF3:{
          get(){
            return this.$store.state.headersF3;
          },
          set(data){
            this.$store.commit('setHeadersF3', data);
          }
      },
      alldata:{
          get(){
            return this.$store.state.alldata;
          },
          set(data){
            this.$store.commit('setAlldata', data);
          }
      },
      filterstatus:{
            get(){
                return this.$store.state.filterstatus;
            },
            set(data){
                this.$store.commit('setFilterstatus', data);
            }
          },
      head:{
          get(){
            //console.log(this.columns);
            return this.columns;
          },
          set(){
            return this.columns;
          }
      },
      
    },
    data(){
     return{
       EXCEL_TYPE : 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;charset=UTF-8',
       EXCEL_EXTENSION : '.xlsx',
       resetFile:{},
       dataFile:{},
       sheets:[],
       files:[],
       search: '',
     } 
    },

    created () {
      this.initialize()
      this.columns=this.headers;
    },

    methods: {
         filterOnlyCapsText (value, search) {
        return value != null &&
          search != null &&
          typeof value === 'string' &&
          (value.toString().toLocaleLowerCase().indexOf(search) !== -1 || 
          value.toString().toLocaleUpperCase().indexOf(search) !== -1|| 
          value.toString().indexOf(search) !== -1)
      },
      initialize () {
        this.tableData = this.alldata ;
      },
      filter(opt){
          switch (opt) {
                case 1:
                    this.search='';
                    this.$store.commit('setFilterstatus', false);
                    this.$store.commit('setColumns', this.headers);
                    break;
                case 2:
                  this.$store.commit('setFilterstatus', false);
                  this.$store.commit('setColumns', this.headersF1);
                    break;
                case 3:
                  this.$store.commit('setFilterstatus', false);
                  this.$store.commit('setColumns', this.headersF2);
                    break;
                case 4:
                  this.$store.commit('setFilterstatus', false);
                  this.$store.commit('setColumns', this.headersF3);
                    break;
                case 5:
                  this.search="sl";
                    break;
              default:
                this.$store.commit('setFilterstatus', false);
                this.$store.commit('setColumns', this.headers);
                  break;
          }
      },
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
        downloadAsExcel(){
          let arrayDownload=[];
          
          this.alldata.forEach(element => {
            let objDownload={};
            this.columns.forEach(element2 => {
              
              objDownload[element2.value] = element[element2.value] ;
              

            });
            arrayDownload.push(objDownload);
          });
          //console.log(arrayDownload);
          const worksheet = XLSX.utils.json_to_sheet(arrayDownload);
          const workbook={
            Sheets:{
              'data':worksheet
            },
            SheetNames:['data']
          };
          const excelBuffer=XLSX.write(workbook,{ bookType:'xlsx', type:'array'});
          //console.log(excelBuffer);
          this.saveAsExcel(excelBuffer, 'myFile');
        },
        saveAsExcel(buffer, filename){
          const data=new Blob([buffer], {type:this.EXCEL_TYPE});
          filesaver.saveAs(data,filename+'_export_'+ new Date().getTime()+this.EXCEL_EXTENSION);
        }
    },
  }
</script>
<style>
  .v-data-table-header th {
    white-space: nowrap;
  }
</style>