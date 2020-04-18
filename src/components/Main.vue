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
                  <button
                    style="border: 1px solid black; margin: 5px; padding: 5px"
                    v-on:click="commitNote(createNote())"
                  >
                    Add note
                  </button>

                  <div v-for="note in notes" :key="note.id">
                    <span>Note</span><br />
                    <span>Id: {{ note.id }}</span
                    ><br />
                    <span>Title: {{ note.title }}</span
                    ><br />
                    <span>Content: {{ note.content }}</span
                    ><br />
                    <span>Tags: {{ note.tags }}</span
                    ><br />
                    <span>When created: {{ note.whenCreated }}</span
                    ><br />
                    <span>When edited: {{ note.whenEdited }}</span
                    ><br /><br />
                  </div>
                  <div>
                    Search phrase:
                    <input
                      style="border: 1px solid black; margin: 5px"
                      v-model="searchPhrase"
                    /><br />
                    <button
                      style="border: 1px solid black; margin: 5px; padding: 5px"
                      v-on:click="searchNotes(searchPhrase)"
                    >
                      Search in content and titles</button
                    ><br />

                    <div>
                      <b-card-group columns>
                        <b-card v-for="note in searchResult" :key="note.id">
                          <b-card-title> {{ note.title }}</b-card-title>
                          <b-card-text>
                            {{ note.content }} <br />
                            ID: {{ note.id }}<br />
                            Tags: {{ note.tags }}<br />
                            When created: {{ note.whenCreated }}<br />
                            When edited: {{ note.whenEdited }}<br />
                          </b-card-text>
                          <b-card-text class="small text-muted"
                            >Last updated 3 mins ago</b-card-text
                          >
                        </b-card>
                      </b-card-group>
                    </div>
                  </div>

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
import { BootstrapVue, IconsPlugin } from "bootstrap-vue";
import "bootstrap/dist/css/bootstrap.css";
import "bootstrap-vue/dist/bootstrap-vue.css";
Vue.use(BootstrapVue);
Vue.use(IconsPlugin);
export default Vue.extend({
  name: "Main",

  data() {
    return {
      id: "",
      content: "",
      tags: "",
      searchPhrase: "",
      searchResult: [],
      groupTags: "",
      groupResults: [],
      drawer: null,
      items: [
        { icon: "fa-search", text: "Notes" },
        { divider: true },
        { heading: "Labels" }
      ]
    };
  },
  computed: {
    ...mapGetters([
      "notes",
      "getNotesCount",
      "getNoteByIndex",
      "getNoteById",
      "searchInContents",
      "searchInTitles",
      "searchInContentsAndTitles",
      "groupByTags",
      "groupByTagsStrict"
    ])
  },
  methods: {
    ...mapActions([
      "resetNotesState",
      "commitNote",
      "commitMultipleNotes",
      "commitDeleteNote",
      "commitEditNote"
    ]),
    getNoteByIndexNumber(index: number) {
      console.log(this.getNoteByIndex(index));
    },
    createNote() {
      return createNote();
    },
    editNoteContent(id: string, content: string) {
      const editableNote = this.getNoteById(id);
      if (editableNote) {
        editableNote.content = content;
        this.commitEditNote(editableNote);
      } else console.log("There is no note with given id");
    },
    editNoteTags(id: string, tags: string) {
      const editableNote = this.getNoteById(id);
      if (editableNote) {
        editableNote.tags = tags.split(",");
        this.commitEditNote(editableNote);
      } else console.log("There is no note with given id");
    },
    searchNotes(phrase: string) {
      this.searchResult = this.searchInContentsAndTitles(phrase);
    },
    getNotesByTags(tags: string) {
      this.groupResults = this.groupByTags(tags.split(","));
    },
    getNotesByTagsStrict(tags: string) {
      this.groupResults = this.groupByTagsStrict(tags.split(","));
    }
  }
});
</script>
