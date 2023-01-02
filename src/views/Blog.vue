<template>
  <div class="blog">
    <router-link to="/" tag="a" class="back">&laquo; Back</router-link>
    <router-view />
    <router-link to="/" tag="a" class="back">&laquo; Back</router-link>
  </div>
</template>
<style lang="scss" scoped>

p {
  margin-top: .4rem;
  font-size: 20px !important;
}

.blog {
  max-width: 90vw;
  margin: 0 auto;

  .back {
    background-color: #42b883;
    color: white;
    margin-top: 2rem;
    text-decoration: none;
    padding: 10px 15px;
    border: 1px solid currentColor;
    border-radius: .5rem;
    display: inline-block;
    transition: all .3s ease;
    &:hover {
      background-color: transparent;
      color: #42b883;
    }
  }

}
</style>
