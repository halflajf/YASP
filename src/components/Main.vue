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

      <!-- <v-content> -->
      <v-container fluid class="grey lighten-4">
        <!-- <v-container fluid fill-height class="grey lighten-4"> -->
        <div>
          <br />
          <br />
          <b-button
            block
            top
            color="brown"
            v-on:click="commitNote(createNote())"
          >
            Generate random note
          </b-button>
          <br />
          <br />
        </div>
        <v-layout justify-center align-center>
          <v-flex shrink>
            <v-tooltip right>
              <template v-slot:activator="{}">
                <div>
                  <b-card-group columns>
                    <b-card v-for="note in notes" :key="note.id">
                      <b-card-title> {{ note.title }}</b-card-title>
                      <b-card-text>
                        {{ note.content }} <br />
                        ID: {{ note.id }}<br />
                        Tags: {{ note.tags }}<br />
                        When created: {{ note.whenCreated }}<br />
                        When edited: {{ note.whenEdited }}<br />
                      </b-card-text>
                      <template v-slot:footer>
                        <small class="text-muted"
                          >Last updated 3 mins ago</small
                        >
                      </template>
                    </b-card>
                  </b-card-group>
                </div>

                <span>Notes goes here!</span>

                //////////////////////////////////////////////////////////

                <div id="my-container">
                  <div class="my-3">
                    <!-- Our triggering (target) element -->
                    <b-button
                      id="popover-reactive-1"
                      variant="primary"
                      ref="button"
                    >
                      Add new note (will be on top)
                    </b-button>
                  </div>

                  <!-- Output from the popover interaction -->
                  <b-card
                    title="Returned values:"
                    v-if="input1Return && input2Return"
                  >
                    <p class="card-text" style="max-width: 20rem;">
                      Name: <strong>{{ input1Return }}</strong
                      ><br />
                      Content: <strong>{{ input2Return }}</strong>
                    </p>
                  </b-card>

                  <!-- Our popover title and content render container -->
                  <!-- We use placement 'auto' so popover fits in the best spot on viewport -->
                  <!-- We specify the same container as the trigger button, so that popover is close to button -->
                  <b-popover
                    target="popover-reactive-1"
                    triggers="click"
                    :show.sync="popoverShow"
                    placement="auto"
                    container="my-container"
                    ref="popover"
                    @show="onShow"
                    @shown="onShown"
                    @hidden="onHidden"
                  >
                    <template v-slot:title>
                      <b-button
                        @click="onClose"
                        class="close"
                        aria-label="Close"
                      >
                        <span class="d-inline-block" aria-hidden="true"
                          >&times;</span
                        >
                      </b-button>
                      New note
                    </template>

                    <div>
                      <b-form-group
                        label="Title"
                        label-for="popover-input-1"
                        label-cols="3"
                        :state="input1state"
                        class="mb-1"
                        invalid-feedback="This field is required"
                      >
                        <b-form-input
                          ref="input1"
                          id="popover-input-1"
                          v-model="input1"
                          :state="input1state"
                          size="sm"
                        ></b-form-input>
                      </b-form-group>

                      <b-form-group
                        label="Content"
                        label-for="popover-input-2"
                        label-cols="3"
                        :state="input1state"
                        class="mb-2"
                      >
                        <b-form-input
                          ref="input2"
                          id="popover-input-2"
                          v-model="input2"
                          :state="input2state"
                          size="sm"
                        >
                        </b-form-input>
                      </b-form-group>

                      <!--
        <b-form-group
          label="Date"
          label-for="datepicker-sm"
          label-cols="3"
          :state="input2state"
          class="mb-2"
          description="Pick a color"
          invalid-feedback="This field is required"
        >
            <b-form-datepicker 
              ref="input2"
              v-model="input2"
              id="datepicker-sm" 
              size="sm" 
              local="en" 
              class="mb-2"
              :state="input2state">            
            </b-form-datepicker>
        </b-form-group>
-->

                      <b-button @click="onClose" size="sm" variant="danger"
                        >Cancel</b-button
                      >
                      <b-button @click="onOk" size="sm" variant="primary"
                        >Ok</b-button
                      >
                    </div>
                  </b-popover>
                </div>
                /////////////////////////////////////////
              </template>
            </v-tooltip>
          </v-flex>
        </v-layout>
      </v-container>

      <!-- </v-content> -->
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
      ],

      input1: "",
      input1state: null,
      input2: "",
      input2state: null,
      input1Return: "",
      input2Return: "",
      popoverShow: false
    };
  },

  watch: {
    input1(val) {
      if (val) {
        this.input1state = true;
      }
    },
    input2(val) {
      if (val) {
        this.input2state = true;
      }
    }
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
    },

    onClose() {
      this.popoverShow = false;
    },
    onOk() {
      if (!this.input1) {
        this.input1state = false;
      }
      if (!this.input2) {
        this.input2state = false;
      }
      if (this.input1 && this.input2) {
        this.onClose();
        // Return our popover form results
        this.input1Return = this.input1;
        this.input2Return = this.input2;
      }
    },
    onShow() {
      // This is called just before the popover is shown
      // Reset our popover form variables
      this.input1 = "";
      this.input2 = "";
      this.input1state = null;
      this.input2state = null;
      this.input1Return = "";
      this.input2Return = "";
    },
    onShown() {
      // Called just after the popover has been shown
      // Transfer focus to the first input
      this.focusRef(this.$refs.input1);
    },
    onHidden() {
      // Called just after the popover has finished hiding
      // Bring focus back to the button
      this.focusRef(this.$refs.button);
    },
    focusRef(ref) {
      // Some references may be a component, functional component, or plain element
      // This handles that check before focusing, assuming a `focus()` method exists
      // We do this in a double `$nextTick()` to ensure components have
      // updated & popover positioned first
      this.$nextTick(() => {
        this.$nextTick(() => {
          (ref.$el || ref).focus();
        });
      });
    }
  }
});
</script>
