<template>
  <div class="lyte-Link">
    <p>{{ link }}</p>
    <div class="lyte-link-menu">
      <img src="@/assets/delete.svg" alt="copy shorten link." />
      <img src="@/assets/copy.svg" alt="copy shorten link." />
    </div>
  </div>
</template>
<script>
export default {
  props: ["link"],
  name: "LyteLink",
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
    img {
      cursor: pointer;
    }
  }
}
</style>
