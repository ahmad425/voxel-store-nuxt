<template>
  <div class="footer-container">
    <div><b>Voxel</b> <span>Store</span></div>
  </div>
</template>

<script>
export default {
  name: 'Footer'
}
</script>

<style lang="scss" scoped>
.footer-container {
  display: flex;
  justify-content: center;
  padding-block: 15px;

  border-top: 1px solid #1A1A1A1A;
  span {
    color: #1A1A1A80;
  }
}
</style>