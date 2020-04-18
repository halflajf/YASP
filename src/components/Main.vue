<template>
  <div id="app">
    <v-app id="inspire">
      <v-app-bar app clipped-left color="#A0522D">
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
        <span class="title ml-3 mr-5">YASP</span>
        <v-text-field
          solo-inverted
          flat
          hide-details
          label="Search"
          prepend-inner-icon=""
        ></v-text-field>
        <v-spacer></v-spacer>
      </v-app-bar>

      <v-navigation-drawer v-model="drawer" app clipped color="grey lighten-4">
        <v-list dense class="grey lighten-4">
          <template v-for="(item, i) in items">
            <v-layout v-if="item.heading" :key="i" row align-center>
              <v-flex xs6>
                <v-subheader v-if="item.heading">
                  {{ item.heading }}
                </v-subheader>
              </v-flex>
              <v-flex xs6 class="text-xs-right">
                <v-btn small text>edit</v-btn>
              </v-flex>
            </v-layout>
            <v-divider
              v-else-if="item.divider"
              :key="i"
              dark
              class="my-3"
            ></v-divider>
            <v-list-item v-else :key="i" v-click="">
              <v-list-item-action>
                <v-icon>{{ item.icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title class="grey--text">
                  {{ item.text }}
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </template>
        </v-list>
      </v-navigation-drawer>

      <v-content>
        <v-container fluid fill-height class="grey lighten-4">
          <v-layout justify-center align-center>
            <v-flex shrink>
              <v-tooltip right>
                <template v-slot:activator="{}">
                  <span>Notes goes here!</span>
                </template>
              </v-tooltip>
              <v-tooltip right> </v-tooltip>
            </v-flex>
          </v-layout>
        </v-container>
      </v-content>
    </v-app>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { mapGetters, mapActions } from "vuex";
import { createNote } from "../models/note.model";

export default Vue.extend({
  name: "Main",

  data: () => ({
    drawer: null,
    items: [
      { icon: "fa-search", text: "Notes" },
      { divider: true },
      { heading: "Labels" }
    ]
  })
});
</script>
