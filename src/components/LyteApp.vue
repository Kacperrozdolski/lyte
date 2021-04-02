<template>
  <div class="lyte-body">
    <section class="lyte-container">
      <div class="lyte-wrapper">
        <div class="lyte-app">
          <h1>Lyte links, big results</h1>
          <h2>
            A URL shortener built meant to help you grow and protect your brand.
          </h2>
          <div class="lyte-input">
            <input
              type="text"
              v-model="url"
              @keydown.enter="console"
              placeholder="Shorten your links"
            />
            <button type="submit" @click="console">Shorten</button>
          </div>
          <div class="lyte-link-container">
            <div class="link-table">
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
            <hr />
            <p>1</p>
          </div>
        </div>
      </div>
      <footer class="lyte-footer">
        <p>copyright © 2021 kacper rozdolski. all rights reserved.</p>
      </footer>
    </section>
    <!-- <div class="lyte-sidebar">
      <img src="@/assets/lyte.svg" alt="" />
    </div> -->
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
    };
  },
  methods: {
    console() {
      let linkRequest = {
        destination: this.url,
        domain: { fullName: "lyte.1amcode.com" },
      };
      let requestHeaders = {
        "Content-Type": "application/json",
        apikey: "ec39449312484443818788162831118e",
      };
      this.axios({
        method: "post",
        url: "https://api.rebrandly.com/v1/links",
        data: JSON.stringify(linkRequest),
        headers: requestHeaders,
        dataType: "json",
      }).then((response) => {
        this.links.push({
          id: this.id++,
          url: response.data.shortUrl,
        });
        document.querySelector("input").value = "asdadssad";
        this.url = "";
      });
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
  flex-direction: row;
  .lyte-container {
    height: 100vh;
    width: 100vw;
    display: flex;
    flex-direction: column;
    align-items: center;
    .lyte-wrapper {
      width: 100%;
      height: 100%;
      display: flex;
      align-items: center;
      flex-direction: column;
      .lyte-app {
        height: 75%;
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
        .lyte-link-container {
          height: 74%;
          .link-table {
            height: 280px;
          }
          hr {
            border: 0;
            height: 2px;
            background: rgb(255, 255, 255);
          }
        }
      }
    }
    .lyte-footer {
      width: 100%;
      height: 10%;
      align-self: flex-end;
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
  .lyte-sidebar {
    height: 100vh;
    width: 30vw;
    background: white;
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>
