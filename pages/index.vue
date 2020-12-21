<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="12" md="12">
      <div class="text-center"></div>
      <v-card>
        <v-tabs
          v-model="tab"
          background-color="deep-purple accent-4"
          centered
          dark
          icons-and-text
        >
          <v-tabs-slider></v-tabs-slider>

          <v-tab href="#tab-1">
            Recents
            <v-icon>mdi-phone</v-icon>
          </v-tab>

          <v-tab href="#tab-2">
            Favorites
            <v-icon>mdi-heart</v-icon>
          </v-tab>

          <v-tab href="#tab-3">
            Nearby
            <v-icon>mdi-account-box</v-icon>
          </v-tab>
        </v-tabs>

        <v-tabs-items v-model="tab">
          <v-tab-item v-for="i in 3" :key="i" :value="'tab-' + i">
            <v-card flat>
              <v-card-text
                ><v-card>
                  <v-system-bar></v-system-bar>
                  <v-toolbar flat>
                    <v-toolbar-title>My Document</v-toolbar-title>
                    <v-spacer></v-spacer>
                    <div>
                      <v-switch
                        v-model="sticky"
                        label="Sticky Banner"
                        hide-details
                      ></v-switch>
                    </div>
                  </v-toolbar>
                  <v-banner single-line :sticky="sticky">
                    We can't save your edits while you are in offline mode.
                    <template v-slot:actions>
                      <v-btn text color="deep-purple accent-4"> Get Online </v-btn>
                    </template>
                  </v-banner>
                  <v-card-text class="grey lighten-4">
                    <v-sheet max-width="800" height="300" class="mx-auto"></v-sheet>
                  </v-card-text> </v-card
              ></v-card-text>
            </v-card>
          </v-tab-item>
        </v-tabs-items>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import Logo from "~/components/Logo.vue";
import VuetifyLogo from "~/components/VuetifyLogo.vue";

export default {
  data() {
    return {
      sticky: false,
      tab: null,
      text:
        "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.",
    };
  },
  components: {
    Logo,
    VuetifyLogo,
  },
};
</script>
