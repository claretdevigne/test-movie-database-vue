<template>
  <div class="app">
    <NavbarVue :addMovie="showAgregarPelicula" :sortMovies="sortMovies" />
    <MoviesTableVue :genres="genres" :peliculas="peliculas" @edit-description="editDescription" @edit-movie="editMovie"
      @delete-movie="deleteMovie" @toggle-favorite="toggleFavorite" />

    <!-- FULL DESCRIPTION MODAL -->
    <v-dialog v-model="selectedMovie" v-if="selectedMovie !== null" width="800">
      <v-card class="dialog-card">
        <v-card-title class="dialog-title">
          <div class="v-card-title-header">
            <div class="movie-title">{{ selectedMovie.title }}</div>
            <v-icon class="close-icon" @click="selectedMovie = null">mdi-close</v-icon>
          </div>
          <v-spacer></v-spacer>
        </v-card-title>

        <v-card-text class="dialog-text" v-if="selectedMovieEditing === false">
          <v-card-text><strong>Backdrop Path:</strong> {{ selectedMovie.backdrop_path }}</v-card-text>
          <v-card-text><strong>Budget:</strong> {{ selectedMovie.budget }}</v-card-text>
          <v-card-text><strong>Genres:</strong>
            <span v-for="genre in selectedMovie.genres" :key="genre.id">{{ genre.name + " " }}</span>
          </v-card-text>
          <v-card-text><strong>Homepage:</strong> {{ selectedMovie.homepage }}</v-card-text>
          <v-card-text><strong>IMDB ID:</strong> {{ selectedMovie.imdb_id }}</v-card-text>
          <v-card-text><strong>Original Language:</strong> {{ selectedMovie.original_language }}</v-card-text>
          <v-card-text><strong>Original Title:</strong> {{ selectedMovie.original_title }}</v-card-text>
          <v-card-text><strong>Overview:</strong> {{ selectedMovie.overview }}</v-card-text>
          <v-card-text><strong>Popularity:</strong> {{ selectedMovie.popularity }}</v-card-text>
          <v-card-text><strong>Poster Path:</strong> {{ selectedMovie.poster_path }}</v-card-text>
          <v-card-text><strong>Production Companies:</strong>
            <ul>
              <li v-for="company in selectedMovie.production_companies" :key="company.id">
                ID: {{ company.id }}, Name: {{ company.name }}, Origin Country: {{ company.origin_country }}
              </li>
            </ul>
          </v-card-text>
          <v-card-text><strong>Production Countries:</strong>
            <ul>
              <li v-for="country in selectedMovie.production_countries" :key="country.iso_3166_1">
                ISO 3166-1: {{ country.iso_3166_1 }}, Name: {{ country.name }}
              </li>
            </ul>
          </v-card-text>
          <v-card-text><strong>Release Date:</strong> {{ selectedMovie.release_date }}</v-card-text>
          <v-card-text><strong>Revenue:</strong> {{ selectedMovie.revenue }}</v-card-text>
          <v-card-text><strong>Runtime:</strong> {{ selectedMovie.runtime }}</v-card-text>
          <v-card-text><strong>Spoken Languages:</strong>
            <ul>
              <li v-for="language in selectedMovie.spoken_languages" :key="language.iso_639_1">
                ISO 639-1: {{ language.iso_639_1 }}, Name: {{ language.name }}
              </li>
            </ul>
          </v-card-text>
          <v-card-text><strong>Status:</strong> {{ selectedMovie.status }}</v-card-text>
          <v-card-text><strong>Tagline:</strong> {{ selectedMovie.tagline }}</v-card-text>
          <v-card-text><strong>Video:</strong> {{ selectedMovie.video }}</v-card-text>
          <v-card-text><strong>Vote Average:</strong> {{ selectedMovie.vote_average }}</v-card-text>
          <v-card-text><strong>Vote Count:</strong> {{ selectedMovie.vote_count }}</v-card-text>
        </v-card-text>

        <v-card-text class="dialog-text" v-if="selectedMovieEditing === true">
          <v-card-text>
            <v-text-field v-model="selectedMovie.backdrop_path" label="Backdrop Path"
              v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.budget" label="Budget" v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-for="genre in selectedMovie.genres" :key="genre.id" v-model="genre.name"
              label="Genre"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.homepage" label="Homepage" v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.imdb_id" label="IMDB ID" v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.original_language" label="Original Language"
              v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.original_title" label="Original Title"
              v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.overview" label="Overview" v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.popularity" label="Popularity"
              v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.poster_path" label="Poster Path"
              v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <strong>Production Companies:</strong>
            <ul>
              <li v-for="company in selectedMovie.production_companies" :key="company.id">
                <v-text-field v-model="company.id" label="ID"></v-text-field>
                <v-text-field v-model="company.name" label="Name"></v-text-field>
                <v-text-field v-model="company.origin_country" label="Origin Country"></v-text-field>
              </li>
            </ul>
          </v-card-text>
          <v-card-text>
            <strong>Production Countries:</strong>
            <ul>
              <li v-for="country in selectedMovie.production_countries" :key="country.iso_3166_1">
                <v-text-field v-model="country.iso_3166_1" label="ISO 3166"></v-text-field>
                <v-text-field v-model="country.name" label="Country name"></v-text-field>
              </li>
            </ul>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.release_date" label="Release Date"
              v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.revenue" label="Revenue" v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.runtime" label="Runtime" v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <strong>Spoken Languages:</strong>
            <ul>
              <li v-for="language in selectedMovie.spoken_languages" :key="language.iso_639_1">
                <v-text-field v-model="language.iso_639_1" label="ISO 639-1"></v-text-field>
                <v-text-field v-model="language.name" label="Language name"></v-text-field>
              </li>
            </ul>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.status" label="Status" v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.tagline" label="Tagline" v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.video" label="Video" v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.vote_average" label="Vote Average"
              v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
          <v-card-text>
            <v-text-field v-model="selectedMovie.vote_count" label="Vote Count"
              v-if="selectedMovieEditing"></v-text-field>
          </v-card-text>
        </v-card-text>

        <v-card-actions v-if="selectedMovieEditing === false">
          <v-btn color="green" text @click="editFullDescription">Editar</v-btn>
          <v-btn color="red" text @click="selectedMovie = null">cerrar</v-btn>
        </v-card-actions>

        <v-card-actions v-if="selectedMovieEditing === true">
          <v-btn color="green" text @click="saveFullDescription">Guardar</v-btn>
          <v-btn color="red" text @click="cancelFullDescription">Cancelar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- EDITING MOVIE MODAL -->
    <v-dialog v-model="editingMovie" v-if="editingMovie !== null" width="800">
      <v-card>
        <v-card-title class="dialog-title">
          <div class="v-card-title-header">
            <span class="movie-title">{{ editingMovie.titulo }}</span>
            <span><v-icon class="close-icon" @click="cancelarEdicion">mdi-close</v-icon></span>
          </div>
          <v-spacer></v-spacer>
        </v-card-title>
        <v-card-text>
          <v-text-field v-model="editingMovie.titulo" label="Título"></v-text-field>
          <v-text-field v-model="editingMovie.genero" label="Género"></v-text-field>
          <v-textarea v-model="editingMovie.overview" label="Overview" :rows="4"></v-textarea>
          <v-text-field v-model="editingMovie.fecha" label="Fecha" type="date"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="green" text @click="guardarCambios">Guardar</v-btn>
          <v-btn color="red" text @click="cancelarEdicion">Cancelar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- ADD MOVIE MODAL -->
    <v-dialog v-model="showAgregarModal" v-if="showAgregarModal === true" width="800">
      <v-card>
        <v-card-title class="dialog-title">
          <div class="v-card-title-header">
            <span class="modal-title">Agregar Película</span>
            <v-icon class="close-icon" @click="showAgregarModal = false">mdi-close</v-icon>
          </div>
          <v-spacer></v-spacer>
        </v-card-title>
        <v-card-text>
          <v-text-field v-model="nuevaPelicula.titulo" label="Título"></v-text-field>
          <v-text-field v-model="nuevaPelicula.genero" label="Género"></v-text-field>
          <v-textarea v-model="nuevaPelicula.overview" label="Overview" :rows="4"></v-textarea>
          <v-text-field v-model="nuevaPelicula.fecha" label="Fecha" type="date"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="green" @click="agregarPelicula">Agregar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import NavbarVue from './components/Navbar.vue';
import MoviesTableVue from './components/MoviesTable.vue';

export default {
  components: {
    NavbarVue,
    MoviesTableVue,
  },
  data() {
    return {
      peliculas: [],
      hoveredRow: null,
      selectedMovie: null,
      selectedMovieEditing: false,
      showEditSelectedMovie: false,
      editingMovie: null,
      showAgregarModal: false,
      genres: [],
      sortBy: null,
      sortDirection: 'asc',
      nuevaPelicula: {
        titulo: '',
        genero: '',
        overview: '',
        fecha: '',
        favorito: false
      }
    };
  },
  created() {

    const options = {
      method: 'GET',
      headers: {
        accept: 'application/json',
        Authorization: process.env.API_KEYS,
      }
    };

    fetch("https://api.themoviedb.org/3/movie/now_playing?language=en-US&page=1", options)
      .then(res => res.json())
      .then(json => {
        this.peliculas = json.results.slice(0, 10).map((movie, index) => ({
          id: index + 1,
          titulo: movie.title,
          genero: movie.genre_ids,
          overview: movie.overview,
          fecha: movie.release_date,
          vote_average: movie.vote_average,
          favorito: false,
          id_movie: movie.id
        }));
      })
      .catch(err => console.error('error:' + err));

    fetch('https://api.themoviedb.org/3/genre/movie/list?language=en', options)
      .then(res => res.json())
      .then(json => this.genres = json)
      .catch(err => console.error('error:' + err));
  },
  methods: {
    sortMovies(sortBy) {
      this.sortBy = sortBy;

      if (this.sortBy === 'title') {
        this.peliculas.sort((a, b) => a.titulo.localeCompare(b.titulo))
      } else if (this.sortBy === 'date') {
        this.peliculas.sort((a, b) => new Date(a.fecha) - new Date(b.fecha));
      } else if (this.sortBy === 'favorites') {
        this.peliculas.sort((a, b) => {
          if (a.favorito && !b.favorito) {
            return -1;
          } else if (!a.favorito && b.favorito) {
            return 1;
          } else {
            return 0;
          }
        });
      }
    },
    toggleFavorite(movie) {
      movie.favorito = !movie.favorito;
      const index = this.peliculas.findIndex(item => item.id === movie.id)
      if (index !== -1) {
        this.peliculas.splice(index, 1, movie)
      }
    },
    editMovie(movie) {
      this.editingMovie = { ...movie };
    },
    cancelarEdicion() {
      this.editingMovie = null;
    },
    guardarCambios() {
      const index = this.peliculas.findIndex(movie => movie.id === this.editingMovie.id);
      this.peliculas.splice(index, 1, this.editingMovie);
      this.editingMovie = null;
    },
    editDescription(movie) {
      const movieId = movie.id_movie;
      const url = `https://api.themoviedb.org/3/movie/${movieId}?language=en-US`;

      const options = {
        method: 'GET',
        headers: {
          accept: 'application/json',
          Authorization: process.env.API_KEY,
        }
      };

      fetch(url, options)
        .then(response => response.json())
        .then(data => {
          this.selectedMovie = { ...data };
        })
        .catch(error => console.log(error));

      this.showEditSelectedMovie = true;
    },
    guardarDescripcion() {
      this.selectedMovie.editing = false;
    },
    cancelarDescripcion() {
      this.selectedMovie.editing = false;
    },
    showAgregarPelicula() {
      this.showAgregarModal = true;
    },

    agregarPelicula() {
      this.showAgregarModal = false;
      this.peliculas = [...this.peliculas, this.nuevaPelicula]
      this.nuevaPelicula = {
        id: this.peliculas.length + 1,
        titulo: '',
        genero: '',
        overview: '',
        fecha: '',
        favorito: false
      }
    },
    deleteMovie(movie) {
      const index = this.peliculas.findIndex(item => item.id === movie.id);
      this.peliculas.splice(index, 1);
    },

    editFullDescription() {
      this.selectedMovieEditing = true
    },

    saveFullDescription() {
      this.selectedMovieEditing = false
    },

    cancelFullDescription() {
      this.selectedMovieEditing = false
    }

  }
};
</script>

<style>
.app {
  width: 100vw;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.movie-title {
  font-size: 24px;
  font-weight: bold;
  margin-right: 10px;
}

.modal-title {
  font-size: 20px;
  font-weight: bold;
}

.v-dialog {
  box-shadow: none !important;
}

.full-description {
  font-size: 16px;
}

.add-movie-button {
  margin-top: 20px;
}

.dialog-card {
  background-color: #f7f7f7;
  border-radius: 8px;
  box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.15);
}

.dialog-title {
  background-color: rgb(145, 85, 110);
  color: #ffffff;
}

.movie-title {
  font-size: 24px;
  font-weight: bold;
}

.close-icon {
  cursor: pointer;
}

.dialog-text {
  margin-top: 16px;
  line-height: 1.5;
}

.dialog-text ul {
  padding-left: 20px;
  list-style-type: disc;
}

/* Estilos responsivos */

@media (max-width: 768px) {
  .app {
    width: auto;
    padding: 0 20px;
  }

  .movie-title {
    font-size: 18px;
  }

  .modal-title {
    font-size: 16px;
  }

  .full-description {
    font-size: 14px;
  }

  .dialog-card {
    border-radius: 4px;
  }
}
</style>

