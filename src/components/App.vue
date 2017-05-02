<template>
  <div id="app">
    <github-badge slug="sinchang/vuemarket" />
    <h1 class="title">Vue Market</h1>
    <p class="subtitle">Fantastic Vue Components and Libraries.</p>
    <nav class="panel">
      <p class="panel-heading">
        Repositories
      </p>
      <div class="panel-block">
        <p class="control has-icons-left">
          <input class="input" type="text" placeholder="Search" v-model="keyword" @input="handleChange">
          <span class="icon is-small is-left">
            <i class="fa fa-search"></i>
          </span>
        </p>
      </div>
      <a class="panel-block" v-for="repo in repos" :href="repo.url" target="_blank">
        <span class="panel-icon">
          <i class="fa fa-book"></i>
        </span> {{ repo.author }}/{{ repo.name }}
        <br> {{ repo.description }}
      </a>
    </nav>
  </div>
</template>

<script>
  import GitHubBadge from 'vue-github-badge'
  import debounce from 'lodash.debounce'
  import repos from '../../data'

  export default {
    name: 'app',
    data() {
      return {
        repos,
        keyword: ''
      }
    },
    methods: {
      handleChange: debounce(function () {
        this.repos = repos
        if (!this.keyword) {
          return
        }
        this.repos = this.repos.filter(repo => {
          return repo.name.toLowerCase().indexOf(this.keyword) > -1 ||
            repo.description.toLowerCase().indexOf(this.keyword) > -1 ||
            repo.tags.indexOf(this.keyword) > -1 ||
            repo.author.toLowerCase().indexOf(this.keyword) > -1
        })
      }, 300)
    },
    components: {
      'github-badge': GitHubBadge
    }
  }
</script>

<style>
  html,
  body,
  #app {
    height: 100%;
  }

  #app {
    margin: 30px auto;
    max-width: 800px;
  }

  .title {
    color: #00d1b2;
    font-weight: 700;
    font-size: 46px;
  }

  .repo-meta {
    color: #00d1b2;
  }

  .panel {
    margin-bottom: 1.5rem;
  }

  body {
    background-color: #fff;
    margin: 0;
  }

  * {
    box-sizing: border-box;
  }

  @media (max-width: 768px) {
    #app {
      width: 90%;
    }
  }
</style>
