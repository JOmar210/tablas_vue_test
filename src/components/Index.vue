<template >
    <v-app class="ma-5">
        <div class="main">
            <div class="ma-3">
    <v-data-table
    :headers="head"
    :items="tableData"
    sort-by="calories"
    class="elevation-1"
    
      :search="search"
      :custom-filter="filterOnlyCapsText"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <!-- <v-toolbar-title>My CRUD</v-toolbar-title> -->
         <v-text-field
          v-model="search"
          label="Search"
          class="mx-4"
        ></v-text-field>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ on, attrs}">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
            >
              New Item
            </v-btn>
              <Popup/>
            <v-btn
              color="primary"
              dark
              class="mb-2 mr-2"
              @click="filter(4)"
            >
              Tier 3
            </v-btn>
            <v-btn
              color="primary"
              dark
              class="mb-2 mr-2"
              @click="filter(3)"
            >
              Tier 2
            </v-btn>
            <v-btn
              color="primary"
              dark
              class="mb-2 mr-2"
              @click="filter(2)"
            >
              Tier 1
            </v-btn>
            <v-btn
              color="primary"
              dark
              class="mb-2 mr-2"
              @click="filter(1)"
            >
              Reset
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.name"
                      label="Dessert name"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.calories"
                      label="Calories"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.fat"
                      label="Fat (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.carbs"
                      label="Carbs (g)"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.protein"
                      label="Protein (g)"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        small
        @click="deleteItem(item)"
        class="mr-2"
      >
        mdi-delete
      </v-icon>
       <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
  </div>
       </div>
    </v-app>
</template>
<script>
import Popup from "./PopupColumns.vue"
  export default {
    components:{Popup},
    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
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
      head(){
          if(this.customHeaders.length > 0&&this.filterstatus==true){
            this.columns=this.customHeaders;
            return this.columns;
            
          }else if(this.filterstatus==false){
            return this.columns;
          }else{
            this.columns=this.headers;
            return this.columns;
          }
          
      },
      
    },
    data(){
     return{
        search: '',
         dialog: false,
      dialogDelete: false,
      customColumns:[],
      columns:[],
      editedIndex: -1,
      editedItem: {
        name: '',
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0,
      },
      defaultItem: {
        name: '',
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0,
      },
     } 
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
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

      editItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },

      deleteItemConfirm () {
        this.desserts.splice(this.editedIndex, 1)
        this.closeDelete()
      },

      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.desserts[this.editedIndex], this.editedItem)
        } else {
          this.desserts.push(this.editedItem)
        }
        this.close()
      },
      filter(opt){
          switch (opt) {
                case 1:
                    this.$store.commit('setFilterstatus', false);
                    this.columns=this.headers;
                    break;
                case 2:
                  this.$store.commit('setFilterstatus', false);
                    this.columns=this.headersF1;
                    break;
                case 3:
                  this.$store.commit('setFilterstatus', false);
                    this.columns=this.headersF2;
                    break;
                case 4:
                  this.$store.commit('setFilterstatus', false);
                    this.columns=this.headersF3;
                    break;
          
              default:
                  break;
          }
      }
    },
  }
</script>
<style>
.v-data-table-header th {
  white-space: nowrap;
}
</style>