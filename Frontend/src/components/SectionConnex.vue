<template>
  <section>
    <article>
      <form @submit.prevent.stop="loginFetch()">
        <p>Connexion</p>
        <div class="row">
          <label for="Name">Pseudo</label>
          <input
            type="text"
            id="Name"
            placeholder="Pseudo"
            pattern="[a-zA-Z0-9]+"
            maxlength="15"
            v-model="dataLogin.email"
          />
        </div>
        <div class="row">
          <label for="pass">Mot de passe</label>
          <input
            type="password"
            id="pass"
            placeholder="Mot de passe"
            v-model="dataLogin.password"
          />
        </div>

        <div>
          <button type="submit" class="btn">Entrer dans la matrice</button>
        </div>
      </form>
    </article>
  </section>
</template>

<script>
export default {
  name: 'SectionConnex',
  data() {
    return {
      dataLogin: {
        email: null,
        password: null,
      },
      msg: '',
    };
  },

  methods: {
    loginFetch() {
      console.log(this.dataLogin);
      fetch('http://localhost:3000/api/user/login', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(this.dataLogin),
      })
        .then((response) => response.json())
        // Then with the data from the response in JSON...
        .then((response) => {
          if (response.token !== undefined) {
            localStorage.setItem('token', response.token);
            localStorage.setItem('userId', response.userId);
            localStorage.setItem('isAdmin', response.isAdmin);
            // eslint-disable-next-line no-restricted-globals
            location.replace(location.origin);
            console.log('Success:', response);
          }
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
}

form {
  text-align: center;
  font-size: 1.2em;
  p {
    font-size: 1.4em;
    font-weight: bold;
  }
  .row {
    display: flex;
    flex-direction: column;
    padding: 10px 10px;
    label {
      margin: 10px;
    }
    input {
      max-width: 80%;
      margin: auto;
      outline: none;
      border: 1px solid #333;
      border-radius: 5px;
      padding: 5px 10px;
    }
  }
  .btn {
    outline: none;
    border: 1px solid #333;
    border-radius: 10px;
    margin: 30px 0;
    padding: 10px 20px;
    cursor: pointer;
    font-size: 20px;
    transition: background-color 0.3s ease-in-out;
    &:hover {
      background-color: #ff3d7f;
      color: white;
    }
  }
}
</style>
