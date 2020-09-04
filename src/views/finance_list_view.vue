<template>
  <v-container fluid grid-list-xs class="pa-0 white">
    <v-row no-gutters class="d-flex justify-center mb-5">
      <v-col cols="12" class="pa-0">
        <v-card class="indigo lighten-2 mx-auto elevation-0" light style="border-radius:0;">
          <div class="d-flex flex-no-wrap justify-space-between">
            <div>
              <v-card-title class="headline white--text">
                R$ {{ this.user_profile.balance}}
              </v-card-title>
            </div>

            <div>
              <v-card-title class="white--text">
                {{ this.user_profile.last_name + ' ' + this.user_profile.first_name}}
              </v-card-title>
            </div>
          </div>
        </v-card>
      </v-col>
    </v-row>
    <v-row no-gutters class="d-flex justify-space-around mb-5">
      <v-col cols="5" class="pa-0">
        <v-card to="transfer" class="mx-auto text-center elevation-0 indigo lighten-2" light>
          <v-icon size="40" color="white">mdi-arrow-top-right-thick</v-icon>
          <v-card-subtitle class="white--text">TRANSFERIR</v-card-subtitle>
        </v-card>
      </v-col>
      <v-col cols="5" class="pa-0">
        <v-card to="transactions" class="mx-auto text-center elevation-0 indigo lighten-2" light>
          <v-icon size="40" color="white">mdi-layers</v-icon>
          <v-card-subtitle class="white--text">TRANSACTIONS</v-card-subtitle>
        </v-card>
      </v-col>
    </v-row>
    <v-row no-gutters class="d-flex justify-space-around">
      <v-col cols="12" class="pa-0">
        <v-card class="mx-auto elevation-0">
          <v-list dense>
            <div class="indigo--text text-center">Últimas Transações</div>
            <v-list-item v-for="(item, i) in profile_list_item" :to="item.to" :key="i">
              <v-list-item-avatar size="35" :color="item.type == 'credit'?'green':'red'">
                <v-icon color="white">{{item.icon}}</v-icon>
              </v-list-item-avatar>
              <v-list-item-content>
                <v-list-item-title class="indigo--text">
                  {{item.text}}
                  <span class="float-right">{{item.time}}</span>
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
          <v-btn to="transactions" text block color="indigo">VER MAIS</v-btn>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import {mapState} from 'vuex';
export default {
  data() {
    return {
      chart_values: Array.from({ length: 10 }, () =>
        Math.floor(Math.random() * 1000)
      ),
      profile_list_item: [
        {
          text: "Newton Salary",
          icon: "mdi-database-plus",
          type: "credit",
          time: "2019/11/04 02:32:43",
          to: "credit_transactions"
        },
        {
          text: "Amazon",
          icon: "mdi-database-minus",
          type: "debit",
          time: "2019/11/04 22:29:11",
          to: "debit_transactions"
        },
        {
          text: "School Fee",
          icon: "mdi-database-minus",
          type: "debit",
          time: "2019/11/04 12:34:35",
          to: "debit_transactions"
        }
      ]
    };
  },
  computed: {
    ...mapState(['current_user', 'user_profile', 'user_bank_details'])
  },
  created(){
    this.load_data()
  },
  methods: {
    load_data(){
      this.$store.dispatch('fetch_user_profile')
      return
    },
  }
};
</script>
