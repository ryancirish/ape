<template>
	<div id="entry_blog_idx" class="">
		<main class="w-80 center tc">
			<div class="cf w-100 helvetica">
				<h1 class="f1">blog</h1>

				<ul class="list cf pl0 pt4-ns">
					<li v-for="article in articles" :key="article.slug" class="grow">
						<NuxtLink :to="{ name: 'blog-slug', params: { slug: article.slug }}">
							<h2 class="f2 lh-title">{{ article.title }}</h2>
						</NuxtLink>
					</li>

				</ul>
			</div>
		</main>
	</div>
</template>

<script>
	// import getSiteMeta from "../../utils/getSiteMeta";

	export default {

	  async asyncData({ $content, params }) {
	    const articles = await $content('articles', params.slug)
	      .only(['title', 'description', 'img', 'slug', 'author'])
	      .sortBy('createdAt', 'asc')
	      .fetch()

	    return {
	      articles
	    }
	  },

	  // head() {
	  // 	return {
	  // 		title: 'tribs blog -- discover your fashion pulse',
	  // 		meta: [
	  // 		  ...this.meta,
	  // 		],
	  // 		link: [
	  // 		  {
	  // 		    hid: "canonical",
	  // 		    rel: "canonical",
	  // 		    href: `https://tri.bs/blog`,
	  // 		  },
	  // 		],
	  // 	}
	  // },

	  computed: {
	    // meta() {
	    //   const metaData = {
	    //     type: 'list',
	    //     title: 'tribs blog -- discover your fashion pulse',
	    //     description: 'Our blog. Intel, interviews, inspiration. Keeping you up to date with everything tribs.',
	    //     url: `${this.$config.baseUrl}/blog/`
	    //   }
	    //   return getSiteMeta(metaData);
	    // }
	  }
	}	
</script>

<style>

</style>