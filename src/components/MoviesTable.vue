<template>
  <div class="movies-table-container">
    <table class="movies-table">
      <thead>
        <tr class="responsive-table-headers">
          <th>Título</th>
          <th>Género</th>
          <th>Overview</th>
          <th class="movies-table-date-column">Fecha</th>
          <th>Votos</th>
          <th class="actions-cell">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr class="responsive-table-content" v-for="movie in peliculas" :key="movie.id" @mouseover="hoveredRow = movie.id" @mouseleave="hoveredRow = null"
          :class="{ 'hovered-row': hoveredRow === movie.id, 'favorite-row': movie.favorito }">
          <td @click.stop="editDescription(movie)">{{ movie.titulo }}</td>
          <td @click.stop="editDescription(movie)">
            <span class="genre-span" v-for="genre in movie.genero" :key="genre">{{ getGenreName(genre) }} </span>
          </td>
          <td @click.stop="editDescription(movie)" class="responsive-movie-table-overview">{{ movie.overview }}</td>
          <td @click.stop="editDescription(movie)" class="movies-table-date-column">{{ movie.fecha }}</td>
          <td @click.stop="editDescription(movie)">{{ movie.vote_average }}</td>
          <td class="actions-cell">
            <v-icon class="action-icon favorite-icon" :class="{ 'favorite-icon-filled': movie.favorito }"
              @click.stop="toggleFavorite(movie)">
              mdi-heart
            </v-icon>
            <v-icon class="action-icon edit-icon" @click.stop="editMovie(movie)">
              mdi-pencil
            </v-icon>
            <v-icon class="action-icon delete-icon" @click.stop="deleteMovie(movie)">
              mdi-delete
            </v-icon>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
  
<script>
export default {
  data() {
    return {
      hoveredRow: null
    };
  },
  props: {
    peliculas: {
      type: Array,
      required: true
    },
    genres: {
      type: Symbol,
      required: true,
    }
  },
  methods: {
    editDescription(movie) {
      this.$emit('edit-description', movie);
    },
    toggleFavorite(movie) {
      this.$emit('toggle-favorite', movie);
    },
    editMovie(movie) {
      this.$emit('edit-movie', movie);
    },
    deleteMovie(movie) {
      this.$emit('delete-movie', movie);
    },
    getGenreName(id) {
      let genreName = ""
      this.genres.genres.filter(item => {
        if (item.id === id) {
          genreName = item.name
        }
      })
      return genreName ? genreName + " " : "Unknown genre"
    },
  }
};
</script>
  
<style>
.movies-table-container {
  width: 100%;
  overflow-x: auto;
  display: flex;
  justify-content: flex-start;
}

.movies-table {
  width: 100%;
  border-collapse: collapse;
}

.movies-table th,
.movies-table td {
  padding: 10px;
  padding-left: 20px;
  padding-right: 20px;
}

.movies-table th {
  background-color: #f2f2f2;
  font-weight: bold;
}

.movies-table tr {
  border-bottom: 1px solid rgb(214, 214, 214)
}

.movies-table-date-column {
  width: 8rem;
}

.movies-table tr:hover {
  background-color: #ffcccc;
  cursor: pointer;
}

.hovered-row {
  background-color: #ffcccc;
}

.actions-cell {
  width: 9rem;
}

.action-icon {
  margin: 0 5px;
  font-size: 20px;
  color: gray;
  cursor: pointer;
}

.action-icon:hover {
  color: black;
}

.favorite-icon-filled {
  color: red !important;
}

.close-icon {
  font-size: 20px;
  color: gray;
  cursor: pointer;
  margin-right: 10px;
}

.v-card-title-header {
  display: flex;
  justify-content: space-between;
}

.genre-span {
  background-color: green;
  color: white;
  margin-left: 3px;
  padding-left: 5px;
  padding-right: 5px;
  border-radius: 10px;
}

/* Estilos responsivos */

@media (max-width: 900px) {
  .responsive-table-content {
    display: flex;
    flex-direction: column;
    width: 100vw;
  }

  .responsive-table-headers {
    display: none;
    width: 0px;
  }

  .responsive-movie-table-overview {
    width: 95vw !important;
    white-space: normal !important;
  }

  .movies-table-container {
    overflow-x: auto;
  }

  .movies-table th,
  .movies-table td {
    white-space: nowrap;
  }

  .movies-table-date-column {
    width: auto;
  }

  .actions-cell {
    width: auto;
  }

  .close-icon {
    margin-right: 5px;
  }
}
</style>

  