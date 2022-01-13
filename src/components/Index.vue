<template >
    <v-app class="ma-5">
        <div class="main">
            <div class="ma-3">
    <v-data-table
    :headers="head"
    :items="alldata"
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
  export default {
    data(){
     return{
         
        search: '',
         dialog: false,
      dialogDelete: false,
      columns:[],
      headers: [{text: "county_name", value: "county_name"},
{text: "county_id", value: "county_id"},
{text: "pid", value: "pid"},
{text: "geo_id", value: "geo_id"},
{text: "account_no", value: "account_no"},
{text: "taxnet_id", value: "taxnet_id"},
{text: "prev_id", value: "prev_id"},
{text: "situs_address", value: "situs_address"},
{text: "situs_num", value: "situs_num"},
{text: "situs_number", value: "situs_number"},
{text: "street_prefix", value: "street_prefix"},
{text: "situs_street", value: "situs_street"},
{text: "street_type", value: "street_type"},
{text: "situs_city", value: "situs_city"},
{text: "situs_state", value: "situs_state"},
{text: "situs_zip", value: "situs_zip"},
{text: "situs_zip_4", value: "situs_zip_4"},
{text: "owner_id", value: "owner_id"},
{text: "owner_name", value: "owner_name"},
{text: "owner_care_of", value: "owner_care_of"},
{text: "owner_addr_1", value: "owner_addr_1"},
{text: "owner_addr_2", value: "owner_addr_2"},
{text: "owner_addr_3", value: "owner_addr_3"},
{text: "owner_city", value: "owner_city"},
{text: "owner_state", value: "owner_state"},
{text: "owner_zip", value: "owner_zip"},
{text: "owner_zip_4", value: "owner_zip_4"},
{text: "abst_subdiv_code", value: "abst_subdiv_code"},
{text: "block", value: "block"},
{text: "tract_lot", value: "tract_lot"},
{text: "building", value: "building"},
{text: "unit", value: "unit"},
{text: "nbhd_code", value: "nbhd_code"},
{text: "acreage", value: "acreage"},
{text: "land_sqft", value: "land_sqft"},
{text: "land_area", value: "land_area"},
{text: "legal_desc", value: "legal_desc"},
{text: "land_value", value: "land_value"},
{text: "improvement_value", value: "improvement_value"},
{text: "market_value", value: "market_value"},
{text: "assessed_value", value: "assessed_value"},
{text: "ag_use_value", value: "ag_use_value"},
{text: "undivided_int", value: "undivided_int"},
{text: "arb_protest_flag", value: "arb_protest_flag"},
{text: "tax_roll_year", value: "tax_roll_year"},
{text: "tax_net_year", value: "tax_net_year"},
{text: "property_type_indicator", value: "property_type_indicator"},
{text: "imprv_sptb", value: "imprv_sptb"},
{text: "land_sptb", value: "land_sptb"},
{text: "pers_sptb", value: "pers_sptb"},
{text: "prod_sptb", value: "prod_sptb"},
{text: "mnrl_sptb", value: "mnrl_sptb"},
{text: "entities", value: "entities"},
{text: "state_entities", value: "state_entities"},
{text: "exemptions", value: "exemptions"},
{text: "deeddate", value: "deeddate"},
{text: "deedbook", value: "deedbook"},
{text: "deedpage", value: "deedpage"},
{text: "num_beds", value: "num_beds"},
{text: "num_baths", value: "num_baths"},
{text: "year_built", value: "year_built"},
{text: "main_area", value: "main_area"},
{text: "total_area", value: "total_area"},
{text: "class_list", value: "class_list"},
{text: "main_class", value: "main_class"},
{text: "type_list", value: "type_list"},
{text: "main_type", value: "main_type"},
{text: "owner_name_search", value: "owner_name_search"},
{text: "company_name", value: "company_name"},
{text: "last_name", value: "last_name"},
{text: "first_name", value: "first_name"},
{text: "second_first", value: "second_first"},
{text: "aux_pid_1", value: "aux_pid_1"},
{text: "aux_pid_2", value: "aux_pid_2"},
{text: "map_id1", value: "map_id1"},
{text: "map_id2", value: "map_id2"},
{text: "centroid_lon", value: "centroid_lon"},
{text: "centroid_lat", value: "centroid_lat"},
{text: "has_pool", value: "has_pool"},
{text: "confidential_owner", value: "confidential_owner"},
{text: "non_res_owner", value: "non_res_owner"},
{text: "isd_state_cd", value: "isd_state_cd"},
{text: "city_state_cd", value: "city_state_cd"},
{text: "taxnet_ptd", value: "taxnet_ptd"},
{text: "sic_code", value: "sic_code"},
{text: "land_hstd_val", value: "land_hstd_val"},
{text: "land_non_hstd_val", value: "land_non_hstd_val"},
{text: "imprv_hstd_val", value: "imprv_hstd_val"},
{text: "imprv_non_hstd_val", value: "imprv_non_hstd_val"},
{text: "ag_market", value: "ag_market"},
{text: "timber_use", value: "timber_use"},
{text: "timber_market", value: "timber_market"},
{text: "appraised_val", value: "appraised_val"},
{text: "ten_percent_cap", value: "ten_percent_cap"},
{text: "agent_code", value: "agent_code"},
{text: "deed_month", value: "deed_month"},
{text: "deed_year", value: "deed_year"},
{text: "prev_val", value: "prev_val"},
{text: "pct_chg", value: "pct_chg"},
{text: "tract_lot_ind", value: "tract_lot_ind"},
{text: "freshness_date", value: "freshness_date"},
{text: "land_cds", value: "land_cds"},
                {text: "land_descrip", value: "land_descrip"}],
      headersF1: [
{text:	"county_name", value: "county_name"},
{text:	"county_id", value: "county_id"},
{text:	"pid", value: "pid"},
{text:	"geo_id", value: "geo_id"},
{text:	"account_no", value: "account_no"},
{text:	"situs_address", value: "situs_address"},
{text:	"situs_num", value: "situs_num"},
{text:	"situs_number", value: "situs_number"},
{text:	"street_prefix", value: "street_prefix"},
{text:	"situs_street", value: "situs_street"},
{text:	"street_type", value: "street_type"},
{text:	"situs_city", value: "situs_city"},
{text:	"situs_state", value: "situs_state"},
{text:	"situs_zip", value: "situs_zip"},
{text:	"situs_zip_4", value: "situs_zip_4"},
{text:	"owner_id", value: "owner_id"},
{text:	"owner_name", value: "owner_name"},
{text:	"owner_care_of", value: "owner_care_of"},
{text:	"owner_addr_1", value: "owner_addr_1"},
{text:	"owner_addr_2", value: "owner_addr_2"},
{text:	"owner_addr_3", value: "owner_addr_3"},
{text:	"owner_city", value: "owner_city"},
{text:	"owner_state", value: "owner_state"},
{text:	"owner_zip", value: "owner_zip"},
{text:	"owner_zip_4", value: "owner_zip_4"},
{text:	"land_value", value: "land_value"},
{text:	"improvement_value", value: "improvement_value"},
{text:	"market_value", value: "market_value"},
{text:	"assessed_value", value: "assessed_value"},
{text:	"ag_use_value", value: "ag_use_value"},
{text:	"tax_roll_year", value: "tax_roll_year"},
{text:	"tax_net_year", value: "tax_net_year"},
{text:	"property_type_indicator", value: "property_type_indicator"},
{text:	"mnrl_sptb", value: "mnrl_sptb"},
{text:	"deeddate", value: "deeddate"},
{text:	"deedbook", value: "deedbook"},
{text:	"deedpage", value: "deedpage"},
{text:	"year_built", value: "year_built"},
{text:	"main_area", value: "main_area"},
{text:	"total_area", value: "total_area"},
{text:	"class_list", value: "class_list"},
{text:	"main_class", value: "main_class"},
{text:	"type_list", value: "type_list"},
{text:	"main_type", value: "main_type"},
{text:	"owner_name_search", value: "owner_name_search"},
{text:	"company_name", value: "company_name"},
{text:	"last_name", value: "last_name"},
{text:	"first_name", value: "first_name"},
{text:	"second_first", value: "second_first"},
{text:	"centroid_lat", value: "centroid_lat"},
{text:	"land_hstd_val", value: "land_hstd_val"},
{text:	"land_non_hstd_val", value: "land_non_hstd_val"},
{text:	"imprv_hstd_val", value: "imprv_hstd_val"},
{text:	"imprv_non_hstd_val", value: "imprv_non_hstd_val"},
{text:	"ag_market", value: "ag_market"},
{text:	"timber_use", value: "timber_use"},
{text:	"timber_market", value: "timber_market"},
{text:	"appraised_val", value: "appraised_val"},
{text:	"ten_percent_cap", value: "ten_percent_cap"},
{text:	"agent_code", value: "agent_code"},
{text:	"deed_month", value: "deed_month"},
{text:	"deed_year", value: "deed_year"},
{text:	"prev_val", value: "prev_val"},
{text:	"pct_chg", value: "pct_chg"},
{text:	"tract_lot_ind", value: "tract_lot_ind"},
{text:	"freshness_date", value: "freshness_date"},
{text:	"land_cds", value: "land_cds"},
{text:	"land_descrip", value: "land_descrip"}
                ],
      headersF2:[{text:	"abst_subdiv_code", value: "abst_subdiv_code"},
                {text:	"block", value: "block"},
                {text:	"tract_lot", value: "tract_lot"},
                {text:	"building", value: "building"},
                {text:	"unit", value: "unit"},
                {text:	"nbhd_code", value: "nbhd_code"},
                {text:	"acreage", value: "acreage"},
                {text:	"land_sqft", value: "land_sqft"},
                {text:	"land_area", value: "land_area"},
                {text:	"legal_desc", value: "legal_desc"},
                {text:	"undivided_int", value: "undivided_int"},
                {text:	"arb_protest_flag", value: "arb_protest_flag"},
                {text:	"entities", value: "entities"},
                {text:	"state_entities", value: "state_entities"},
                {text:	"exemptions", value: "exemptions"},
                {text:	"num_beds", value: "num_beds"},
                {text:	"num_baths", value: "num_baths"},
                {text:	"centroid_lon", value: "centroid_lon"},
                {text:	"has_pool", value: "has_pool"},
                {text:	"isd_state_cd", value: "isd_state_cd"},
                {text:	"city_state_cd", value: "city_state_cd"},
                {text:	"taxnet_ptd", value: "taxnet_ptd"},
                {text:	"sic_code", value: "sic_code"}],
      headersF3:[{text:	"imprv_sptb", value:"imprv_sptb"},
                {text:	"land_sptb", value:"land_sptb"},
                {text:	"pers_sptb", value:"pers_sptb"},
                {text:	"prod_sptb", value:"prod_sptb"},
                {text:	"confidential_owner", value:"confidential_owner"},
                {text:	"non_res_owner", value:"non_res_owner"}],
      desserts: [],
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

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
      head(){
          return this.columns;
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
        this.alldata = [
              {
    county_name	:'denton',
    county_id	:'61',
    pid	:'36328',
    geo_id	:'A0328A-000-0031-0000',
    account_no	:'',
    taxnet_id	:'36328',
    prev_id	:'',
    situs_address	:' ',
    situs_num	:'',
    situs_number	:'0',
    street_prefix	:'',
    situs_street	:'',
    street_type	:'',
    situs_city	:'DENTON',
    situs_state	:'TX',
    situs_zip	:'76208',
    situs_zip_4	:'',
    owner_id	:'936391',
    owner_name	:'KOAN SCHOOL',
    owner_care_of	:'',
    owner_addr_1	:'',
    owner_addr_2	:'PO BOX 2961',
    owner_addr_3	:'',
    owner_city	:'DENTON',
    owner_state	:'TX',
    owner_zip	:'76202',
    owner_zip_4	:'2961',
    abst_subdiv_code	:'A0328A',
    block	:'',
    tract_lot	:'31',
    building	:'',
    unit	:'',
    nbhd_code	:'EX11.21',
    acreage	:'2',
    land_sqft	:'87120',
    land_area	:'2',
    legal_desc	:'A0328A W. DABBS, TR 31, 2.0 ACRES',
    land_value	:'48506',
    improvement_value	:'0',
    market_value	:'48506',
    assessed_value	:'48506',
    ag_use_value	:'0',
    undivided_int	:'',
    arb_protest_flag	:'F    ',
    tax_roll_year	:'2021',
    tax_net_year	:'2021',
    property_type_indicator	:'R',
    imprv_sptb	:'',
    land_sptb	:'E4',
    pers_sptb	:'',
    prod_sptb	:'',
    mnrl_sptb	:'',
    entities	:'CAD, G01, S05',
    state_entities	:'061-000-00, 061-CAD-XX, 061-901-02',
    exemptions	:'',
    deeddate	:'8012016',
    deedbook	:'',
    deedpage	:'',
    num_beds	:'',
    num_baths	:'',
    year_built	:'0',
    main_area	:'0',
    total_area	:'0',
    class_list	:'',
    main_class	:'',
    type_list	:'',
    main_type	:'',
    owner_name_search	:'KOAN SCHOOL',
    company_name	:'',
    last_name	:'Koan',
    first_name	:'School',
    second_first	:'',
    aux_pid_1	:'363449',
    aux_pid_2	:'R36328',
    map_id1	:'',
    map_id2	:'',
    centroid_lon	:'-97.05738441',
    centroid_lat	:'33.24095551',
    has_pool	:'N    ',
    confidential_owner	:'N    ',
    non_res_owner	:'Y    ',
    isd_state_cd	:'061-901-02',
    city_state_cd	:'',
    taxnet_ptd	:'E',
    sic_code	:'',
    land_hstd_val	:'0',
    land_non_hstd_val	:'48506',
    imprv_hstd_val	:'0',
    imprv_non_hstd_val	:'0',
    ag_market	:'0',
    timber_use	:'0',
    timber_market	:'0',
    appraised_val	:'48506',
    ten_percent_cap	:'0',
    agent_code	:'0',
    deed_month	:'8',
    deed_year	:'2016',
    prev_val	:'48506',
    pct_chg	:'0',
    tract_lot_ind	:'',
    freshness_date	:'10/26/2021',
    land_cds	:'PN 3',
    land_descrip	:'NATIVE PASTURE III',
    },
    {
    county_name	:'denton',
    county_id	:'61',
    pid	:'7268',
    geo_id	:'SL0245A-00000C-0000-0013-0000',
    account_no	:'',
    taxnet_id	:'7268',
    prev_id	:'',
    situs_address	:' SELLMEYER LN',
    situs_num	:'',
    situs_number	:'0',
    street_prefix	:'',
    situs_street	:'SELLMEYER',
    street_type	:'LN',
    situs_city	:'',
    situs_state	:'TX',
    situs_zip	:'',
    situs_zip_4	:'',
    owner_id	:'673443',
    owner_name	:'ONCOR ELECTRIC DELIVERY CO',
    owner_care_of	:'',
    owner_addr_1	:'UNIT',
    owner_addr_2	:'PO BOX 139100',
    owner_addr_3	:'',
    owner_city	:'DALLAS',
    owner_state	:'TX',
    owner_zip	:'75313',
    owner_zip_4	:'9100',
    abst_subdiv_code	:'SL0245A',
    block	:'C',
    tract_lot	:'13 & 14',
    building	:'',
    unit	:'',
    nbhd_code	:'',
    acreage	:'1.3912',
    land_sqft	:'0',
    land_area	:'0',
    legal_desc	:'VILLAGE ESTATES 2 BLK C LOT 13 & 14, Undivided Interest 50.1000000000%',
    land_value	:'3206',
    improvement_value	:'0',
    market_value	:'3206',
    assessed_value	:'3206',
    ag_use_value	:'0',
    undivided_int	:'',
    arb_protest_flag	:'F    ',
    tax_roll_year	:'2021',
    tax_net_year	:'2021',
    property_type_indicator	:'R',
    imprv_sptb	:'',
    land_sptb	:'J3',
    pers_sptb	:'',
    prod_sptb	:'',
    mnrl_sptb	:'',
    entities	:'C08, CAD, G01, S09',
    state_entities	:'061-CAD-XX, 061-902-02, 061-104-03, 061-000-00',
    exemptions	:'',
    deeddate	:'1072002',
    deedbook	:'',
    deedpage	:'',
    num_beds	:'',
    num_baths	:'',
    year_built	:'0',
    main_area	:'0',
    total_area	:'0',
    class_list	:'',
    main_class	:'',
    type_list	:'',
    main_type	:'',
    owner_name_search	:'ONCOR ELECTRIC DELIVERY CO',
    company_name	:'ONCOR ELECTRIC DELIVERY CO',
    last_name	:'',
    first_name	:'',
    second_first	:'',
    aux_pid_1	:'181810',
    aux_pid_2	:'R07268',
    map_id1	:'',
    map_id2	:'',
    centroid_lon	:'0',
    centroid_lat	:'0',
    has_pool	:'N    ',
    confidential_owner	:'N    ',
    non_res_owner	:'Y    ',
    isd_state_cd	:'061-902-02',
    city_state_cd	:'061-104-03',
    taxnet_ptd	:'J3',
    sic_code	:'',
    land_hstd_val	:'3206',
    land_non_hstd_val	:'0',
    imprv_hstd_val	:'0',
    imprv_non_hstd_val	:'0',
    ag_market	:'0',
    timber_use	:'0',
    timber_market	:'0',
    appraised_val	:'3206',
    ten_percent_cap	:'0',
    agent_code	:'0',
    deed_month	:'1',
    deed_year	:'2002',
    prev_val	:'0',
    pct_chg	:'0',
    tract_lot_ind	:'',
    freshness_date	:'10/26/2021',
    land_cds	:'',
    land_descrip	:'',
    },
    {
    county_name	:'denton',
    county_id	:'61',
    pid	:'7281',
    geo_id	:'SL0245A-00000K-0000-0000-0001',
    account_no	:'',
    taxnet_id	:'7281',
    prev_id	:'',
    situs_address	:' DELMAR PL',
    situs_num	:'',
    situs_number	:'0',
    street_prefix	:'',
    situs_street	:'DELMAR',
    street_type	:'PL',
    situs_city	:'HIGHLAND VILLAGE',
    situs_state	:'TX',
    situs_zip	:'75077',
    situs_zip_4	:'',
    owner_id	:'673443',
    owner_name	:'ONCOR ELECTRIC DELIVERY CO',
    owner_care_of	:'',
    owner_addr_1	:'UNIT',
    owner_addr_2	:'PO BOX 139100',
    owner_addr_3	:'',
    owner_city	:'DALLAS',
    owner_state	:'TX',
    owner_zip	:'75313',
    owner_zip_4	:'9100',
    abst_subdiv_code	:'SL0245A',
    block	:'K',
    tract_lot	:'',
    building	:'',
    unit	:'',
    nbhd_code	:'',
    acreage	:'1.5496',
    land_sqft	:'0',
    land_area	:'0',
    legal_desc	:'VILLAGE ESTATES 2 BLK K LOT 4-7,8(S PT OF LT), Undivided Interest 50.1000000000%',
    land_value	:'5357',
    improvement_value	:'0',
    market_value	:'5357',
    assessed_value	:'5357',
    ag_use_value	:'0',
    undivided_int	:'',
    arb_protest_flag	:'F    ',
    tax_roll_year	:'2021',
    tax_net_year	:'2021',
    property_type_indicator	:'R',
    imprv_sptb	:'',
    land_sptb	:'J3',
    pers_sptb	:'',
    prod_sptb	:'',
    mnrl_sptb	:'',
    entities	:'C08, CAD, G01, S09',
    state_entities	:'061-104-03, 061-902-02, 061-CAD-XX, 061-000-00',
    exemptions	:'',
    deeddate	:'1072002',
    deedbook	:'',
    deedpage	:'',
    num_beds	:'',
    num_baths	:'',
    year_built	:'0',
    main_area	:'0',
    total_area	:'0',
    class_list	:'',
    main_class	:'',
    type_list	:'',
    main_type	:'',
    owner_name_search	:'ONCOR ELECTRIC DELIVERY CO',
    company_name	:'ONCOR ELECTRIC DELIVERY CO',
    last_name	:'',
    first_name	:'',
    second_first	:'',
    aux_pid_1	:'181812',
    aux_pid_2	:'R07281',
    map_id1	:'',
    map_id2	:'',
    centroid_lon	:'-97.04942938',
    centroid_lat	:'33.07886788',
    has_pool	:'N    ',
    confidential_owner	:'N    ',
    non_res_owner	:'Y    ',
    isd_state_cd	:'061-902-02',
    city_state_cd	:'061-104-03',
    taxnet_ptd	:'J3',
    sic_code	:'',
    land_hstd_val	:'0',
    land_non_hstd_val	:'5357',
    imprv_hstd_val	:'0',
    imprv_non_hstd_val	:'0',
    ag_market	:'0',
    timber_use	:'0',
    timber_market	:'0',
    appraised_val	:'5357',
    ten_percent_cap	:'0',
    agent_code	:'0',
    deed_month	:'1',
    deed_year	:'2002',
    prev_val	:'0',
    pct_chg	:'0',
    tract_lot_ind	:'',
    freshness_date	:'10/26/2021',
    land_cds	:'',
    land_descrip	:'',
    },
        ]
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
                    this.columns=this.headers;
                    break;
                case 2:
                    this.columns=this.headersF1;
                    break;
                case 3:
                    this.columns=this.headersF2;
                    break;
                case 4:
                    this.columns=this.headersF3;
                    break;
          
              default:
                  break;
          }
      }
    },
  }
</script>