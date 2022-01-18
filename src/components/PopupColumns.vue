<template>
  <v-dialog
      v-model="dialog"
      scrollable
      persistent
      max-width="400px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="primary"
          dark
          v-bind="attrs"
          v-on="on"
          class="mr-2 mb-2"
        >
          Select Columns
        </v-btn>
      </template>
      <v-card>
        <v-card-title>Select Columns</v-card-title>
        <v-divider></v-divider>
        <v-card-text style="height: 400px; ">
          <v-select
          v-model="Columns2"
          :items="ColumnData"
          label="Select Columns"
          multiple
          chips
          persistent-hint
        ></v-select>
        </v-card-text>
        <v-divider></v-divider>
        <v-card-actions>
          <v-btn
            color="blue darken-1"
            text
            @click="dialog = false"
          >
            Close
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="CleanFilter"
          >
            Clean
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="customFilter"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
</template>
<script>
  export default {
      computed:{
          headers:{
            get(){
                return this.$store.state.headers;
            },
            set(data){
                this.$store.commit('setHeaders', data);
            }
          },
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
          filterstatus:{
            get(){
                return this.$store.state.filterstatus;
            },
            set(data){
                this.$store.commit('setFilterstatus', data);
            }
          },
      },
    data () {
      return {
        dialogm1: '',
        dialog: false,
        Columns2: [],
        ColumnData: ['county_name','county_id','pid','geo_id','account_no','taxnet_id','prev_id','situs_address','situs_num','situs_number','street_prefix','situs_street','street_type','situs_city','situs_state','situs_zip','situs_zip_4','owner_id','owner_name','owner_care_of','owner_addr_1','owner_addr_2','owner_addr_3','owner_city','owner_state','owner_zip','owner_zip_4','abst_subdiv_code','block','tract_lot','building','unit','nbhd_code','acreage','land_sqft','land_area','legal_desc','land_value','improvement_value','market_value','assessed_value','ag_use_value','undivided_int','arb_protest_flag','tax_roll_year','tax_net_year','property_type_indicator','imprv_sptb','land_sptb','pers_sptb','prod_sptb','mnrl_sptb','entities','state_entities','exemptions','deeddate','deedbook','deedpage','num_beds','num_baths','year_built','main_area','total_area','class_list','main_class','type_list','main_type','owner_name_search','company_name','last_name','first_name','second_first','aux_pid_1','aux_pid_2','map_id1','map_id2','centroid_lon','centroid_lat','has_pool','confidential_owner','non_res_owner','isd_state_cd','city_state_cd','taxnet_ptd','sic_code','land_hstd_val','land_non_hstd_val','imprv_hstd_val','imprv_non_hstd_val','ag_market','timber_use','timber_market','appraised_val','ten_percent_cap','agent_code','deed_month','deed_year','prev_val','pct_chg','tract_lot_ind','freshness_date','land_cds','land_descrip'],
      }
    },
    methods:{
        customFilter(){
            this.$store.commit('setFilterstatus', true);
            this.customColumns=[];
            this.$store.commit('setCustomHeaders', this.customColumns);
            this.Columns2.forEach(element => {
                this.customColumns.push({"text": element, "value": element});
            });
            this.$store.commit('setCustomHeaders', this.customColumns);
            console.log(this.customHeaders);
            if(this.customHeaders.length > 0 ){
            this.$store.commit('setColumns', this.customHeaders);
            
            }else{
            this.$store.commit('setColumns', this.headers);
            } 
            this.dialog = false;
        },
        CleanFilter(){
         this.Columns2=[];   
        }
    }
  }
</script>