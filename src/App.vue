<template>
  <v-app>
    <v-navigation-drawer fixed clipped app v-model="drawer">
      <v-list dense>
        <v-layout row align-center :key="i">
          <v-flex xs6>
            <v-subheader>Sessions</v-subheader>
          </v-flex>
          <v-flex xs6 class="text-xs-right">
            <v-btn small flat>edit</v-btn>
          </v-flex>
        </v-layout>

        <v-list-tile @click="drawer=!drawer">
          <v-list-tile-action>
            <v-icon class="cubing-icon event-333"/>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>Main</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>

        <v-list-tile @click="">
          <v-list-tile-action>
            <v-icon>add</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>Add Session</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>

      </v-list>
    </v-navigation-drawer>

    <v-toolbar dark app clipped-left fixed>
      <v-toolbar-side-icon @click.stop="drawer = !drawer"/>
      <v-toolbar-title>vTimer</v-toolbar-title>
    </v-toolbar>

    <v-content>
      <v-container fluid fill-height>
        <router-view/>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      drawer: true,
    };
  },
};
</script>

<style>

</style>
