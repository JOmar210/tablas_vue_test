<template>
  <div>
    <v-app-bar
      color="deep-purple accent-4"
      dense
      dark
      height="80px"
    >
    <v-row align="center">
      <v-col><v-img 
        src="https://images.squarespace-cdn.com/content/v1/5d6c197f8a813e0001fd4ea7/1567373639160-CHZJP9CAPMPYYQNOBC2O/JJC-Full-Logo-Gold.png?format=1500w"
        max-height="150"
        max-width="250"
        @click="homeRouter"
        style="cursor: pointer;"
        ></v-img>
        </v-col>
        <v-col class="pagesBTN pt-6 pb-6">
          <div  @click="homeRouter"><h3>Table</h3></div>
        </v-col>
        <v-col class="pagesBTN pt-6 pb-6">
          <div  @click="importRouter"><h3>Import</h3></div>
        </v-col>
        <v-col class="pagesBTN pt-6 pb-6">
          <div  @click="downloadRouter"><h3>Download</h3></div>
        </v-col>
    </v-row>
        

      <v-spacer></v-spacer>
      <v-menu
        left
        bottom
      >
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            icon
            v-bind="attrs"
            v-on="on"
          >
            <v-icon>mdi-account</v-icon>
          </v-btn>
        </template>

        <v-list>
          <v-list-item>
            <v-list-item-title>Profile</v-list-item-title>
          </v-list-item>
          <v-list-item>
            <v-list-item-title>Settings</v-list-item-title>
          </v-list-item>
          <v-list-item>
            <v-list-item-title>Log out</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-app-bar>
  </div>
</template>
<script>
export default {
  data(){
    return{
      //
    }
  },
  methods:{
    homeRouter(){
        this.$router.push("/");
      },
      importRouter(){
        this.$router.push("/import");
      },
      downloadRouter(){
        this.$router.push("/download");
      },
  }
}
</script>
<style>

.pagesBTN:hover{
  background-color: rgb(0, 0, 0,.3);
  cursor: pointer;
}
h3{
  text-align: center;
}
</style>