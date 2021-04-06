<template>
  <div class="lyte-body">
    <section class="lyte-container">
      <div class="lyte-app">
        <div class="lyte-description">
          <h1>Lyte links, big results</h1>
          <h2>
            A URL shortener built meant to help you grow and protect your brand.
          </h2>
        </div>
        <div class="lyte-input">
          <input
            type="text"
            v-model="url"
            @keydown.enter="createLink"
            placeholder="Shorten your links"
          />
          <button type="submit" @click="createLink">Shorten</button>
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
      <footer></footer>
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
            this.url = "";
          })
          .catch(() => {
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
  flex-direction: column;
  .lyte-container {
    min-height: 100vh;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    .lyte-app {
      display: flex;
      flex-direction: column;
      min-height: 90vh;
      .lyte-description {
        h1 {
          font-size: 4.5rem;
          margin: 40px 0;
          filter: drop-shadow(5px 5px 0px #f68fff);
        }
        h2 {
          font-size: 1.5rem;
          margin: 40px 0;
          font-weight: 100;
        }
      }
      .lyte-input {
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
          background: rgb(206, 206, 206);
        }
      }
      .lyte-link-table {
        color: white;
      }
    }
    footer {
      height: 10vh;
      width: 100%;
      background: red;
      align-self: flex-end;
    }
  }
}
</style>
