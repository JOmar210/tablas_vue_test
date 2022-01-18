<template>
  <v-app>
    <v-content>
        <v-alert
      :value="alert"
      color="pink"
      dark
      border="top"
      icon="mdi-account"
      transition="scale-transition"
    >
      No user registered
      </v-alert>
      <v-card width="500" class="mx-auto mt-9">
        <v-card-title>LOG-IN</v-card-title>
        <v-card-text>
          <v-text-field 
          v-model="user1"
          label="Username" 
          prepend-icon="mdi-account-circle"/>
          <v-text-field 
          v-model="pass"
          label="Password" 
          :type="showPassword ? 'text' : 'password'"
          prepend-icon="mdi-lock"
          :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
          @click:append="showPassword = !showPassword"/>
        </v-card-text>
        <v-card-actions>
          <!-- <v-btn color="success">Register</v-btn> -->
          <v-row>
              <v-col cols="12" class="d-flex justify-center" >
                  <v-btn color="info" class="px-4 text-h6" 
                   @click="login">Enter</v-btn>
              </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
      
    </v-content>
  </v-app>
</template>
<script>
export default {
    computed: {
      users:{
          get(){
            return this.$store.state.users;
          },
          set(data){
            this.$store.commit('setUsers', data);
          }
      },
    },
    data()
    {
        return{
        showPassword:false,
        alert:false,
        user1:'',
        pass:'',
        }
    },
    methods: {
        login(){
            this.users.forEach(element => {
                if(element.user==this.user1 && element.password==this.pass){
                    this.$router.push("/");
                    this.alert = false;
                }
            });
            
            this.alert = true;
            
        }
    },
}
</script>