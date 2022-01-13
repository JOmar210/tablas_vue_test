<template>
  <div>
    <v-app-bar
      color="deep-purple accent-4"
      dense
      dark
      height="80px"
    >
        <v-img 
        src="https://images.squarespace-cdn.com/content/v1/5d6c197f8a813e0001fd4ea7/1567373639160-CHZJP9CAPMPYYQNOBC2O/JJC-Full-Logo-Gold.png?format=1500w"
        max-height="150"
        max-width="250"
        ></v-img>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-heart</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

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
            <v-icon>mdi-dots-vertical</v-icon>
          </v-btn>
        </template>

        <v-list>
          <v-list-item
            v-for="n in 5"
            :key="n"
            @click="() => {}"
          >
            <v-list-item-title>Option {{ n }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-app-bar>
  </div>
</template>