<template>
  <section>
    <article id="card-container">
      <div class="empty" v-if="Gifs.length === 0">
        <img src="../assets/giphy.gif" alt="Pulp Fiction" />
        <h2>Pas de Gif disponible !</h2>
      </div>
      <!-- Faire la carte ici -->
      <div v-for="gif in Gifs" :key="gif.id" class="card">
        <router-link
          :to="'/gif-modification?id=' + gif.id"
          class="cardBody"
          @:click="tokenGif()"
        >
          <div class="userName">
            <p>{{ gif.user.email }}</p>
          </div>
          <div class="gif">
            <h2>{{ gif.title }}</h2>
            <img :src="gif.imageUrl" alt="Gif" />
            <p>{{ gif.description }}</p>
          </div>
        </router-link>
      </div>
    </article>
  </section>
</template>

<script>
export default {
  name: 'Section',

  data() {
    return {
      Gifs: [],
    };
  },
  created() {
    this.gifFetch();
  },

  methods: {
    gifFetch() {
      fetch('http://localhost:3000/api/gif', {
        method: 'GET',
        headers: {
          authorization: `Bearer ${localStorage.getItem('token')}`,
          'Content-Type': 'application/json',
        },
      })
        .then((response) => response.json())
        .then((response) => {
          this.Gifs = response;
          // console.log(response)
        })
        // Then with the error genereted...
        .catch((error) => {
          console.error('Error:', error);
        });
    },
  },
};
</script>

<style lang="scss">
@mixin tablet {
  @media all and (max-width: 700px) {
    @content;
  }
}

section {
  display: flex;
  flex-grow: 1;
  flex-shrink: 0;
}

article {
  background-color: rgba($color: #c6e5d9, $alpha: 1);
  width: 50%;
  margin: 0 auto;
  padding: 20px 0;
  z-index: 1;

  @include tablet {
    width: 80%;
  }
  .empty {
    display: flex;
    flex-direction: column;
    img {
      max-width: 500px;
      margin: 40px auto;
      border-radius: 2%;
    }
    h2 {
      margin: 10px auto;
      color: black;
    }
  }
  .card {
    display: block;
    text-align: center;
    width: 50%;
    margin: 0 auto 60px;
    padding: 5px 0 10px;
    background-color: lighten($color: #c6e5d9, $amount: 8%);
    border-radius: 5%;

    @include tablet {
      width: 80%;
    }

    .cardBody {
      text-decoration: none;
      color: black;

      .userName {
        text-align: left;
        margin: 0 10px -15px;
        color: grey;
        font-size: 0.8em;
      }

      .gif {
        margin: 0 0 10px;
        h2 {
          font-size: 1.3em;
        }
        img {
          max-width: 90%;
          height: auto;
          border-radius: 2%;
        }
      }
    }
  }
}
</style>
