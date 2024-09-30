<template>
  <div class="welcome-container">
    <h3>
      Welcome to Voxel
    </h3>
    <p>Discover our wide range of products</p>
  </div>
</template>

<script>
export default {
  name: 'Welcome',
  
}
</script>

<style lang="scss" scoped>
.welcome-container {
  padding-block: 10rem;
  display: flex;
  flex-direction: column;
  align-items: center;

  p {
    color: #1A1A1A80;
  }
}
</style>