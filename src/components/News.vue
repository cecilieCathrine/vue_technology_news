<template>
  <div>
    <form>
      <input
        type="search"
        id="search"
        name="search"
        aria-label="Search"
        v-model="queryString"
        placeholder="Search for technology news..."
        @keyup="fetchNews()"
      />
      <input
        v-model="language"
        id="us"
        type="radio"
        name="language"
        value="us"
        @change="fetchNews()"
      />
      <label for="us">English (US)</label>
      &nbsp;
      <input
        v-model="language"
        id="no"
        type="radio"
        name="language"
        value="no"
        @change="fetchNews()"
      />
      <label for="en">Norwegian</label>
    </form>
    <div v-if="articles.length > 0" class="news_articles">
      <div
        v-for="article in articles"
        :key="article.name"
        class="news_article"
        @click="goToArticle(article.url)"
      >
        <h3>{{ article.title }}</h3>
        <p>
          <img width="200px" :src="article.urlToImage" :alt="article.name" />
          {{ article.description }}
        </p>
      </div>
    </div>
    <div v-else>
      <Message message="No articles found" />      
   </div>
  </div>
</template>

<script>
import Message from "./Message";
export default {
  name: "News",
  components: {
    Message
  },
  data() {
    return {
      language: "no",
      articles: [],
      queryString: ""
    };
  },
  created() {
    this.fetchNews();
  },
  methods: {
    fetchNews() {
      const queryString = this.queryString
        ? `q=${this.queryString}&country=${this.language}&category=technology`
        : `country=${this.language}&category=technology`;
      const url = `https://newsapi.org/v2/top-headlines?${queryString}`;
      fetch(url, {
        method: "GET",
        headers: {
          "X-Api-Key": "aa9fb7c4a9164fdb909ede436b864e00"
        }
      })
        .then(response => response.json())
        .then(json => {
          this.articles = json.articles;
        });
    },
    goToArticle(url) {
      window.open(url, "_blank");
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.news_articles {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  width: 100%;
  margin-bottom: 3em;
}
.news_article {
  display: flex;
  flex-direction: column;
  cursor: pointer;
  width: 40%;
  margin: 2em;
  border-left: 2px solid #008cba;
  border-top: 2px solid #008cba;
  padding-top: 1em;
  padding-left: 1em;
  background: white;
  color: black;
}
.news_article:hover {
  background: #f4f4f4;
}
img {
  float: left;
  padding-right: 1em;
}
p {
  text-align: left;
}
input[type="search"] {
  border: 1px solid #008cba;
  font-size: 1em;
  padding: 10px;
  width: 16em;
  margin-right: 0.5em;
  margin-bottom: 1em;
}
</style>
