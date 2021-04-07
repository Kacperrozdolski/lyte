<template>
  <div class="lyte-Link">
    <p>{{ link }}</p>
    <div class="lyte-link-menu">
      <div class="delete">
        <img
          src="@/assets/delete.svg"
          alt="copy shorten link."
          @click="deleteLink($event)"
        />
      </div>
      <div class="copy">
        <img
          src="@/assets/copy.svg"
          alt="copy shorten link."
          @click="copyLink($event)"
        />
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["link"],
  name: "LyteLink",
  methods: {
    deleteLink($event) {
      let id = $event.path[2].id;
      this.$emit("deleteLink", id);
    },
    copyLink($event) {
      let id = $event.path[2].id;
      this.$emit("copyLink", id);
    },
  },
};
</script>

<style lang="scss">
.lyte-Link {
  display: flex;
  align-items: center;
  justify-content: space-between;
  p {
    font-weight: 200;
  }
  .lyte-link-menu {
    width: 60px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    .delete {
      display: flex;
      align-items: center;
      position: relative;
      img {
        cursor: pointer;
      }
    }
    .copy {
      display: flex;
      align-items: center;
      position: relative;
      img {
        cursor: pointer;
      }
    }
  }
}
</style>
