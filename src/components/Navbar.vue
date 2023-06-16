<template>
  <div class="navbar">
    <span></span>
    <span class="navbar-title">Movie Database</span>
    <span class="navbar-buttons">
      <v-menu>
        <template v-slot:activator="{ props }">
          <v-btn color="primary" v-bind="props"> Order By </v-btn>
        </template>
        <v-list>
          <v-list-item v-for="option in orderByOptions" :key="option.value" @click="handleOrderBy(option.value)">
            <v-list-item-title>{{ option.label }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
      <v-btn color="red" @click.stop="handleAddMovie()">Add Movie</v-btn>
    </span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      orderByOptions: [
        { label: 'By title', value: 'title' },
        { label: 'By date', value: 'date' },
        { label: 'By favorite', value: 'favorites' },
      ],
    };
  },
  methods: {
    handleAddMovie() {
      this.addMovie();
    },
    handleOrderBy(option) {
      this.sortMovies(option)
    },
  },
  props: {
    addMovie: {
      type: Function,
      required: true,
    },

    sortMovies: {
      type: Function,
      required: true,
    },
  },
};
</script>

<style scoped>
.navbar {
  background-color: rgb(145, 85, 110);
  padding: 0 2rem;
  height: 60px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.navbar-title {
  color: white;
  font-weight: bold;
  font-size: 18px;
}

.navbar-buttons {
  display: flex;
  gap: 1rem;
}

/* Estilos responsivos */

@media (max-width: 768px) {
  .navbar {
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: auto;
    padding: 1rem;
  }

  .navbar-buttons {
    margin-top: 1rem;
    gap: 0.5rem;
  }
}
</style>

