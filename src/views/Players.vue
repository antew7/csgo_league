<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="3" lg="3" sm="6" xs="6" justify="center" align="center">
        <v-switch v-model="people" label="Gold" value="Gold" color="#91ff00"></v-switch>
      </v-col>
      <v-col cols="12" md="3" lg="3" sm="6" xs="6" justify="center" align="center">
        <v-switch v-model="people" label="Silver" value="Silver" color="#91ff00"></v-switch>
      </v-col>
      <v-col cols="12" md="3" lg="3" sm="6" xs="6" justify="center" align="center">
        <v-switch v-model="people" label="Bronze" value="Bronze" color="#91ff00"></v-switch>
      </v-col>
      <v-col cols="12" md="3" lg="3" sm="6" xs="6" justify="center" align="center">
        <v-switch v-model="people" label="All" value="All" color="#91ff00"></v-switch>
      </v-col>
    </v-row>

    <v-container grid-list-sm align-content-space-between>
      <v-layout v-if="this.people == 'All'" wrap>
          <v-flex v-for="(ex,k) in this.players" :key="k" xs12 sm6 md3>
            <Player :data="ex" />
          </v-flex>
        </v-layout>
        <v-layout v-if="this.people == 'Silver'" wrap>
          <v-flex v-for="(ex,k) in this.silver" :key="k" xs12 sm6 md3>
            <Player :data="ex" />
          </v-flex>
        </v-layout>
        <v-layout v-if="this.people == 'Gold'" wrap>
          <v-flex v-for="(ex,k) in this.gold" :key="k" xs12 sm6 md3>
            <Player :data="ex" />
          </v-flex>
        </v-layout>
        <v-layout v-if="this.people == 'Bronze'" wrap>
          <v-flex v-for="(ex,k) in this.bronze" :key="k" xs12 sm6 md3>
            <Player :data="ex" />
          </v-flex>
        </v-layout>
    </v-container>
  </v-container>
</template>

<script>
import firebase from "firebase/app";
import "firebase/firestore";
import db from "../firebase";
import Player from "../components/Player";

export default {
  name: "players",
  components: {
    Player
  },
  created() {
    db.firestore()
      .collection("players")
      .orderBy("name")
      .get()
      .then(res => {
        res.forEach(doc => {
          this.players.push(doc.data());
          // console.log(doc.data());
          if (doc.data().category == "Gold") {
            this.gold.push(doc.data());
          }
          if (doc.data().category == "Silver") {
            this.silver.push(doc.data());
          }
          if (doc.data().category == "Bronze") {
            this.bronze.push(doc.data());
          }
        });
      });
  },
  data() {
    return {
      players: [],
      gold: [],
      silver: [],
      bronze: [],
      people: "All"
    };
  }
};
</script>