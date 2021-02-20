<template>
	<!-- debug-grid debug-black -->
	<div id="entry_blog" class="">
		<nav class="center w-50-ns tc">
			<h1 class="helvetica f-subheadline dib mb2 mb4-ns">🦍</h1>
		</nav>
		<article class="helvetica w-80 center pb6 pt4">
			<div class="w-100 cf">
				<h1 class="mr3 f-subheadline pl4-ns tc">{{ article.title }}</h1>
				<p class="f2 ph3-ns lh-copy tc">{{ article.description }}</p>
<!-- 				<div class="fr-ns w-50-ns ph3-ns">
					<img :src="article.img" :alt="article.alt" />
				</div>
				<div class="fl-ns w-50-ns">
					<h1 class="mr3 f1 pl4-ns">{{ article.title }}</h1>
					<p class="f3 ph3-ns measure lh-copy">{{ article.description }}</p>
				</div> -->
			</div>
			<nuxt-content class="w-90 f3 center pt4 lh-copy" :document="article" />
		</article>
  	</div>
</template>

<script>
  // import getSiteMeta from "../../utils/getSiteMeta";

  export default {
    async asyncData({ $content, params }) {
      const article = await $content('articles', params.slug).fetch()

      return { article }
    },

    // head() {
    // 	return {
    // 		title: this.article.title,
		  //   meta: [
		  //     ...this.meta,
		  //     {
		  //       property: "article:published_time",
		  //       content: this.article.createdAt,
		  //     },
		  //     {
		  //       property: "article:modified_time",
		  //       content: this.article.updatedAt,
		  //     },
		  //     {
		  //       property: "article:tag",
		  //       content: this.article.tags ? this.article.tags.toString() : "",
		  //     },
		  //     { name: "twitter:label1", content: "Written by" },
		  //     { name: "twitter:data1", content: "Ryan" },
		  //     { name: "twitter:label2", content: "Filed under" },
		  //     {
		  //       name: "twitter:data2",
		  //       content: this.article.tags ? this.article.tags.toString() : "",
		  //     },
		  //   ],
		  //   link: [
		  //     {
		  //       hid: "canonical",
		  //       rel: "canonical",
		  //       href: `https://tri.bs/blog/${this.$route.params.slug}`,
		  //     },
		  //   ],
    // 	};
    // },

    // computed: {
    //   meta() {
    //     const metaData = {
    //       type: "article",
    //       title: this.article.title,
    //       description: this.article.description,
    //       url: `${this.$config.baseUrl}/articles/${this.$route.params.slug}`,
    //       mainImage: this.article.image,
    //     };
    //     return getSiteMeta(metaData);
    //   }
    // }
  }
</script>

<style>
	.fnt {
		font-family: "Mincho", serif;
	}

	.measure-75 {
		width: 75%;
		margin: 0 auto;
	}
</style>