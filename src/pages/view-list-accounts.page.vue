<template>
  <q-page class="q-pa-lg">
    <title>Manage Accounts | Lanao Map</title>
    <div v-if="position == 'Provincial Admin'"></div>
    <q-card v-if="position == 'Municipal Admin'">
      <q-tabs
        v-model="tab"
        dense
        class="text-primary"
        active-color="primary"
        indicator-color="primary"
        align="justify"
        style="font-family: ubuntu"
      >
        <q-tab name="list" label="Pending Account List" no-caps />
        <q-tab name="grid" label="Confirmed Account List" no-caps />
      </q-tabs>

      <q-separator />

      <q-tab-panels v-model="tab">
        <q-tab-panel name="list" class="q-pa-none">
          <PendingAccountListMA />
        </q-tab-panel>

        <q-tab-panel name="grid" class="q-pa-none">
          <ConfirmedAccountListMA />
        </q-tab-panel>
      </q-tab-panels>
    </q-card>

    <q-card v-else-if="position == 'Provincial Admin'" class="shadow-5">
      <TableListForPA />
    </q-card>
  </q-page>
</template>

<script>
import PendingAccountListMA from "./Municipal Admin/list-pending-accounts.table";
import ConfirmedAccountListMA from "./Municipal Admin/list-confirmed-accounts.table";
import TableListForPA from "./Provincial Admin/list-table-accounts.table";

import * as firebase from "firebase/app";
import "firebase/auth";
import db from "../Firestore/firebaseInit";
export default {
  components: {
    PendingAccountListMA,
    ConfirmedAccountListMA,
    TableListForPA,
  },

  data() {
    return {
      tab: "list",

      position: null,
    };
  },

  created() {
    this.culturalHeritages = true;
    var user = firebase.auth().currentUser;
    if (user) {
      db.collection("profiles")
        .doc(user.uid)
        .onSnapshot(
          (doc) => {
            this.position = doc.data().position;
          },
          (err) => {
            // console.log(err.message)
          }
        );
    }
  },

  computed: {
    culturalHeritages: {
      get() {
        return this.$store.state.siteNav.culturalHeritages;
      },

      set(val) {
        this.$store.dispatch("siteNav/culturalHeritages", val);
      },
    },
  },
};
</script>

<style></style>
