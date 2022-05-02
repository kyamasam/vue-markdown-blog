<template>
  <div class="home">
    <div class="container">
      <h5 class="headline center">A blog by Samfast1</h5>
    </div>
    <div class="container">
      <a href="https://twitter.com/kyamasam" class="container" target="_blank">
        <img alt="Twitter Icon" class="container-item" src="@/assets/twitter-icon.svg"/> <span class="container-item">Twitter</span>
      </a>
      <a href="https://github.com/kyamasam" class="container" target="_blank">
        <img alt="Github Icon" class="container-item" src="@/assets/github-icon.svg"/> <span class="container-item">Github</span>
      </a>
    </div>

    <div class="sections">
      <div v-for="(section, index) in Object.keys(entries)" :key="index" class="group">
        <h2 class="center">{{section}}</h2>
        <div class="section" v-for="entry in entries[section]" :key="entry.id">
          <div class="entry">
            <h3 @click="$router.push({name: entry.id})">
              {{entry.title}}
              <span class="subtitle">{{entry.date}}</span>
            </h3>
            <p>{{entry.description}}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BLOGENTRIES from '@/statics/data/blogs.json'

export default {
  name: 'home',
  computed: {
    entries() {
      return BLOGENTRIES
    }
  }
}
</script>
<style lang="scss" scoped>
.container{
  display: flex;
  justify-content: center;
  align-items: center;
}
.container-item{
  margin: 2px;
}
.center {
  text-align: center;
}
.headline {
  text-transform: uppercase;
  margin: 2rem;
  font-size: 3rem;
}
img {
  display: inline-block;
  margin: 0 auto;
  width: 20px;
}

h2 {
  color: #35495e;
  text-transform: capitalize;
  margin-bottom: 2rem;
}

h3 {
  color: #42b883;
  margin-bottom: 0;
  cursor: pointer;
  &:hover {
    text-decoration: underline;
  }
  .subtitle {
    color: grey;
    font-size: .98rem;
    float: right;
    font-weight: normal;
  }
}

p {
  margin-top: .4rem;
}

.sections {
  max-width: 70vw;
  margin: 0 auto;
  margin-top: 4rem;
}

.section {
  margin-bottom: 3rem;
}

.group {
  margin-bottom: 4rem;
}

</style>
