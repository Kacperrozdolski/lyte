<template>
  <div class="lyte-body">
    <img src="@/assets/lyte.png" class="logo" />
    <section class="lyte-container">
      <div class="lyte-app">
        <div class="lyte-description">
          <h1>Lyte links, big results</h1>
          <h2>
            A URL shortener built meant to help you grow and strengthen your
            links.
          </h2>
        </div>
        <div class="lyte-input">
          <label class="slideInUp" ref="errorLabel" for="link"
            >invalid link, try again</label
          >
          <input
            id="link"
            type="text"
            v-model="url"
            @keydown.enter="createLink"
            placeholder="Shorten your links"
          />
          <button v-if="!isLoading" type="submit" @click="createLink">
            Shorten
          </button>
          <div v-if="isLoading" class="loader-container">
            <div class="loader"></div>
          </div>
        </div>
        <div class="lyte-link-table">
          <component
            :is="'LyteLink'"
            v-for="link in links"
            :key="link.id"
            :id="link.id"
            :link="link.url"
            @deleteLink="deleteLink"
            @copyLink="copyLink"
          ></component>
        </div>
      </div>
      <footer>
        <p>copyright © 2021 kacper rozdolski. all rights reserved.</p>
        <div class="footer-wrapper">
          <a href="mailto:leopoldsinx@gmail.com"
            ><img src="@/assets/mail.svg"
          /></a>
          <a
            href="https://www.linkedin.com/in/kacper-rozdolski-ba6a4715b/"
            target="_blank"
            ><img src="@/assets/linkedin.svg"
          /></a>
          <a href="https://github.com/Kacperrozdolski" target="_blank"
            ><img src="@/assets/github.svg"
          /></a>
        </div>
      </footer>
    </section>
  </div>
</template>

<script>
import LyteLink from "./LyteLink.vue";
export default {
  name: "HelloWorld",
  components: { LyteLink },
  data() {
    return {
      url: "",
      links: [],
      id: 0,
      isLoading: false,
    };
  },
  methods: {
    createLink() {
      if (this.isLoading == false) {
        let linkRequest = {
          destination: this.url,
          domain: { fullName: "lyte.1amcode.com" },
        };
        let requestHeaders = {
          "Content-Type": "application/json",
          apikey: "ec39449312484443818788162831118e",
        };
        this.isLoading = true;
        console.log("Wysłam", "Ładuje się? " + this.isLoading);

        this.axios({
          method: "post",
          url: "https://api.rebrandly.com/v1/links",
          data: JSON.stringify(linkRequest),
          headers: requestHeaders,
          dataType: "json",
        })
          .then((response) => {
            this.links.push({
              id: this.id++,
              url: response.data.shortUrl,
            });
            this.isLoading = false;
            console.log("Odebrałem", "Ładuje się? " + this.isLoading);
            this.$refs.errorLabel.classList.remove("visible");
            this.url = "";
          })
          .catch(() => {
            this.$refs.errorLabel.classList.add("visible");
            this.isLoading = false;
            console.log("Odebrałem", "Ładuje się? " + this.isLoading);
          });
      }
    },
    copyLink(id) {
      let link = this.links.find((link) => link.id == id);
      let index = this.links.indexOf(link);
      navigator.clipboard.writeText(this.links[index].url);
    },
    deleteLink(id) {
      let link = this.links.find((link) => link.id == id);
      let index = this.links.indexOf(link);
      this.links.splice(index, 1);
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;800&display=swap");

.lyte-body {
  min-height: 100vh;
  width: 100%;
  background: black;
  display: flex;
  justify-content: center;
  overflow: auto;
  position: relative;
  .logo {
    position: absolute;
    height: 600px;
    z-index: 2;
    animation: floating 2s infinite;
    animation-direction: alternate-reverse;
    animation-fill-mode: forwards;
    animation-timing-function: ease-in-out;
  }
  @keyframes floating {
    0% {
      top: 0px;
    }

    100% {
      top: 30px;
    }
  }
  @keyframes error {
    0% {
      left: 50%;
    }
    50% {
      right: 30px;
    }
    100% {
      left: 0;
      right: 0;
    }
  }
  .lyte-container {
    min-height: 50vh;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    z-index: 3;
    overflow: hidden;
    .lyte-app {
      display: flex;
      flex-direction: column;
      min-height: 80vh;
      .lyte-description {
        h1 {
          font-size: 4.5rem;
          margin: 40px 0;
          text-align: center;
          filter: drop-shadow(5px 5px 0px #00af91);
        }
        h2 {
          font-size: 1.5rem;
          margin: 40px 0;
          font-weight: 100;
          text-align: center;
        }
      }
      .lyte-input {
        position: relative;
        display: flex;
        label {
          position: absolute;
          top: -25px;
          color: rgb(255, 0, 0);
          font-weight: 300;
          display: none;
        }
        input {
          width: 78%;
          height: 40px;
          margin: 0;
          padding: 0 0 0 2%;
          border: 0;
          outline: 0;
        }
        button {
          width: 20%;
          height: 40px;
          padding: 0;
          margin: 0;
          border: 0;
          outline: 0;
          cursor: pointer;
        }
        button:hover {
          background: rgb(202, 202, 202);
        }
        .loader-container {
          width: 20%;
          height: 40px;
          background: rgb(239, 239, 239);
          display: flex;
          justify-content: center;
          align-items: center;
          padding: 0;
          margin: 0;
          border: 0;
          outline: 0;
          .loader {
            border: 5px solid #ffffff;
            border-radius: 50%;
            border-top: 5px solid #00af91;
            width: 20px;
            height: 20px;
            -webkit-animation: spin 2s linear infinite; /* Safari */
            animation: spin 2s linear infinite;
          }
          @keyframes spin {
            0% {
              -webkit-transform: rotate(0deg);
            }
            100% {
              -webkit-transform: rotate(360deg);
            }
          }
        }
      }
      .lyte-link-table {
        color: white;
      }
    }
    footer {
      min-height: 150px;
      width: 100%;
      color: #e7d9ea;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      img {
        margin: 10px;
      }
      p {
        text-align: center;
      }
    }
  }
}
.visible {
  display: block !important;
}

@media only screen and (max-width: 600px) {
  .lyte-app {
    width: 95%;
    .lyte-description {
      h1 {
        font-size: 3rem !important;
      }
      h2 {
        font-size: 1rem !important;
      }
    }
  }
}
</style>
