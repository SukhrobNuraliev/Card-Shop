<template>
  <v-container style="max-width: 1100px">
    <v-btn
      @click="showButton"
      :loading="loading"
      :disabled="loading"
      block
      large
      :class="[showForm ? 'red white--text' : 'success']"
    >
      {{ showForm ? "Cancel" : "Create Card" }}
    </v-btn>
    <v-card v-if="showForm" elevation="2" style="margin-top: 5px">
      <v-card-text>
        <form @submit.prevent="createCard">
          <v-text-field v-model="form.title" label="Title"></v-text-field>
          <v-textarea v-model="form.content" label="Content"></v-textarea>

          <v-btn class="mr-4 success" type="submit"> Create </v-btn>
        </form>
      </v-card-text>
    </v-card>
    <v-row v-if="loadingCards">
      <v-col v-for="(load, index) in loadingArray" :key="index" cols="12" md="4" sm="6">
        <v-skeleton-loader
          style="margin-top: 2.5rem"
          type="image, article"
        ></v-skeleton-loader>
      </v-col>
    </v-row>

    <v-row>
      <v-col v-for="card in cards" :key="card.id" cols="12" md="4" sm="6">
        <v-card
          style="margin-bottom: 1px !important"
          class="mx-auto my-12 k-card"
          max-width="374"
        >
          <nuxt-link
            class="card-link"
            :to="{ name: 'cards-id', params: { id: card.id } }"
          >
            <v-img
              height="250"
              src="https://cdn.vuetifyjs.com/images/cards/cooking.png"
            ></v-img>
          </nuxt-link>
          <v-card-title>
            <nuxt-link
              class="card-link"
              :to="{ name: 'cards-id', params: { id: card.id } }"
            >
              {{ card.title }}
            </nuxt-link>
          </v-card-title>

          <v-card-text>
            <v-row align="center" class="mx-0">
              <v-rating
                :value="rating"
                color="amber"
                background-color="grey"
                dense
                hover
                half-increments
                size="14"
              ></v-rating>

              <div class="grey--text ml-4">4.5</div>
            </v-row>

            <div>
              {{ card.content }}
            </div>
          </v-card-text>

          <v-divider class="mx-4"></v-divider>

          <v-card-actions>
            <v-btn
              :to="{ name: 'cards-id', params: { id: card.id } }"
              color="deep-purple lighten-2"
              text
            >
              Explore
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      loader: null,
      loading: false,
      loadingCards: false,
      loadingArray: [1, 1, 1, 1, 1, 1],
      showForm: false,
      rating: 4.5,
      loading: false,
      selection: 1,
      cards: [],
      form: {
        title: "",
        content: "",
      },
    };
  },
  inject: {
    theme: {
      default: { isDark: false },
    },
  },
  mounted() {
    this.loadingCards = true;
    this.$http.$get("/cards").then((res) => {
      this.cards = res;
      this.loadingCards = false;
    });
  },
  methods: {
    showButton() {
      this.showForm = !this.showForm;
      this.form = {};
    },
    createCard() {
      this.loader = "loading";
      this.loading = true;
      this.$http.$post("/cards", this.form).then((res) => {
        console.log(res);
        this.cards.unshift(res);
        this.loading = false;
        this.loader = null;
        this.form = {};
        this.showForm = false;
      });
    },
  },
};
</script>

<style scoped>
.card-link {
  color: rgba(0, 0, 0, 0.87);
  text-decoration: none;
}
.custom-loader {
  animation: loader 1s infinite;
  display: flex;
}
@keyframes loader {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
