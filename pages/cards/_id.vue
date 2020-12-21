<template>
  <v-row>
    <div class="mx-auto">
      <v-col cols="12">
        <v-btn @click="goBack" color="grey darken-4" dark
          ><v-icon left> mdi-arrow-left </v-icon>Back</v-btn
        >
        <v-card class="" max-width="">
          <v-img
            class="white--text align-end"
            height="500px"
            src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
          >
            <v-card-title>{{ card.title }}</v-card-title>
          </v-img>

          <v-card-subtitle class="pb-0"> ID {{ card.id }} </v-card-subtitle>

          <v-card-text class="text--primary">
            <div>{{ card.content }}</div>
          </v-card-text>

          <v-card-actions>
            <v-dialog v-model="dialog" persistent max-width="600px">
              <template v-slot:activator="{ on, attrs }">
                <v-btn color="primary" dark v-bind="attrs" v-on="on"> Edit </v-btn>
              </template>
              <v-card>
                <form @submit.prevent="updateCard">
                  <v-card-title>
                    <span class="headline">Change Card</span>
                  </v-card-title>
                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="12">
                          <v-text-field v-model="card.title" label="Title"></v-text-field>
                          <v-textarea v-model="card.content" label="Content"></v-textarea>
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="dialog = false">
                      Cancel
                    </v-btn>
                    <v-btn color="blue darken-1" text type="submit"> Update </v-btn>
                  </v-card-actions>
                </form>
              </v-card>
            </v-dialog>

            <v-btn
              @click="deleteCard"
              color="deep-orange accent-4"
              dark
              style="margin-left: 4px"
            >
              Delete
            </v-btn>
            <v-snackbar v-model="snackbar">
              {{ text }}

              <template v-slot:action="{ attrs }">
                <v-btn color="pink" text v-bind="attrs" @click="snackbar = false">
                  Close
                </v-btn>
              </template>
            </v-snackbar>
          </v-card-actions>
        </v-card>
      </v-col>
    </div>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      dialog: false,
      card: {},
      snackbar: false,
      text: "Hello, I'm a snackbar",
    };
  },
  methods: {
    goBack() {
      this.$router.go(-1);
    },
    updateCard() {
      this.$http.$put("/cards/" + this.$route.params.id, this.card).then((res) => {
        console.log(res);
        this.dialog = false
      });
    },
    deleteCard() {
      this.$http.$delete("/cards/" + this.$route.params.id).then(() => {
        this.snackbar = true;
        this.$router.push({ name: "cards" });
      });
    },
  },
  created() {
    this.$http.$get("/cards/" + this.$route.params.id).then((res) => {
      this.card = res[0];
    });
  },
};
</script>

<style scoped>
.grey {
  margin-bottom: 1rem;
}
</style>
