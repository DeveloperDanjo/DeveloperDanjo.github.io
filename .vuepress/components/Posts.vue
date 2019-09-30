<template>
  <div class="posts" v-if="posts.length">
    <div class="post" v-for="post in posts">
        <img v-if="post.frontmatter.image" :src="$withBase(post.frontmatter.image)" alt="">
        <img v-else src="/img/blog.svg">
        <div>
            <router-link :to="post.path">{{post.frontmatter.title}}</router-link>
            <div>{{post.frontmatter.description}}</div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["page", "limit"],
  computed: {
    posts() {
      let currentPage = this.page ? this.page : this.$page.path;
      let posts = this.$site.pages
        .filter(x => {
          return x.path.match(new RegExp(`(${currentPage})(?=.*html)`));
        })
        .sort((a, b) => {
          return new Date(b.frontmatter.date) - new Date(a.frontmatter.date);
        });

      if (this.limit != null) { return posts.slice(0, this.limit); }
      return posts;
    }
  }
};
</script>

<style lang="stylus" scoped>
.posts {
  margin-bottom: 1em;
}
.post {
    display: flex;
    
    img {
        height: 64px;
        width: 64px;
        margin-right: 16px;
        border-radius: 4px;
    }

    a {
        display: block;
        font-size: 1.4rem;
        margin-bottom: 0.5em;
    }
}
</style>