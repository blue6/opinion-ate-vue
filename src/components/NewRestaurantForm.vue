<template>
  <form @submit.prevent="handleSave">
    <v-alert
      v-if="validationError"
      type="error"
      data-testid="new-restaurant-name-error"
    >
      Name is required.
    </v-alert>
    <v-alert
      v-if="serverError"
      type="error"
      data-testid="new-restaurant-server-error"
    >
      The restaurant could not be saved. Please try again.
    </v-alert>
    <v-row>
      <v-col cols="9">
        <v-text-field
          v-model="name"
          data-testid="new-restaurant-name-field"
          placeholder="Add Restaurant"
          filled
          type="text"
        />
      </v-col>
      <v-col cols="3">
        <v-btn
          data-testid="new-restaurant-submit-button"
          type="submit"
          color="primary"
          class="black--text"
          block
        >
          Add
        </v-btn>
      </v-col>
    </v-row>
  </form>
</template>

<script>
import {mapActions} from 'vuex';

export default {
  name: 'NewRestaurantForm',
  data() {
    return {
      name: '',
      validationError: false,
      serverError: false,
    };
  },
  methods: {
    ...mapActions({
      createRestaurant: 'restaurants/create',
    }),
    handleSave() {
      if (this.name) {
        this.serverError = false;
        this.validationError = false;
        this.createRestaurant(this.name)
          .then(() => {
            this.name = '';
          })
          .catch(() => {
            this.serverError = true;
          });
      } else {
        this.validationError = true;
      }
    },
  },
};
</script>
