<template>
  <section>
    <article id="card-container">
      <form @submit="userModifyFetch()" id="inscriForm">
        <p>Profil Utilisateur</p>
        <div class="row">
          <label for="Name">Pseudo</label>
          <input
            type="text"
            id="Name"
            placeholder="Pseudo"
            pattern="[a-zA-Z0-9]+"
            maxlength="15"
            v-model="dataUpdate.email"
          />
        </div>
        <div class="row">
          <label for="pass">Mot de passe</label>
          <input
            type="password"
            id="pass"
            placeholder="Mot de passe"
            v-model="dataUpdate.password"
          />
        </div>

        <div>
          <button type="submit" class="btn">Mise à jour</button>
        </div>
      </form>

      <div class="DecoDelete">
        <button type="submit" class="btnDecoDelete" @click="userDeco()">
          Déconnexion
        </button>
        <button type="submit" class="btnDecoDelete" @click="userDelete()">
          Suppression du compte
        </button>
      </div>
    </article>
  </section>
</template>

<script>
export default {
  name: 'SectionProfil',
  data() {
    return {
      dataUpdate: {
        email: null,
        password: null,
      },
      msg: '',
    };
  },

  methods: {
    userModifyFetch() {
      console.log(this.dataUpdate);
      fetch(
        `http://localhost:3000/api/user/${localStorage.getItem('userId')}`,
        {
          method: 'PUT',
          headers: {
            authorization: `Bearer ${localStorage.getItem('token')}`,
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(this.dataUpdate),
        },
      )
        .then((response) => response.json())
        // Then with the data from the response in JSON...
        .then((user) => {
          console.log('Success:', user);
        })
        // Then with the error genereted...
        .catch((error) => {
          console.error('Error:', error);
        });
    },

    userDeco() {
      localStorage.clear();
      window.location.reload();
    },

    userDelete() {
      fetch(
        `http://localhost:3000/api/user/${localStorage.getItem('userId')}`,
        {
          method: 'DELETE',
          headers: {
            authorization: `Bearer ${localStorage.getItem('token')}`,
            'Content-Type': 'application/json',
          },
        },
      )
        .then((response) => response.json())
        // Then with the data from the response in JSON...
        .then((user) => {
          console.log('Success:', user);
          localStorage.clear();
          window.location.reload();
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
    margin: 30px 0 10px;
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

.DecoDelete {
  display: flex;
  flex-direction: row;
  justify-content: center;

  @include tablet {
    flex-direction: column;
  }
  .btnDecoDelete {
    outline: none;
    display: block;
    border: 1px solid #333;
    border-radius: 10px;
    margin: 60px 30px 0;
    padding: 10px 20px;
    cursor: pointer;
    font-size: 20px;
    transition: background-color 0.3s ease-in-out;

    @include tablet {
      margin: 40px auto;
    }
    &:hover {
      background-color: #ff3d7f;
      color: white;
    }
  }
}
</style>
