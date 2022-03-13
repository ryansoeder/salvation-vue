<template>
	<template v-if="pageInfo">
		<h1 v-if="pageInfo.title" :key="pageInfo.title.rendered">
			{{ pageInfo.title.rendered }}
		</h1>
		<transition name="fade">
			<div v-if="blocks" class="blocks">
				<component
					:is="blockCaller(block.acf_fc_layout)"
					:block="block"
					v-for="block in blocks"
					:key="block.title"
				/>
			</div>
		</transition>
	</template>
	<template v-else>
		<NotFound />
	</template>
</template>

<script>
import { defineAsyncComponent } from '@vue/runtime-core';
import NotFound from '@/components/NotFound.vue';

export default {
	name: 'Page',
	components: {
		NotFound,
	},
	data() {
		return {
			pageInfo: null,
			blocks: null,
		};
	},
	methods: {
		async fetchPage() {
			const res = await fetch(
				`http://tattoo-salvation.local/wp-json/wp/v2/pages?slug=${this.$route.params.slug}}`
			);
			const page = await res.json();
			this.pageInfo = await page[0];
			this.blocks = await this.pageInfo.acf.blocks;
		},
		blockCaller(componentname) {
			return defineAsyncComponent(() =>
				import(`@/components/blocks/${componentname}.vue`)
			);
		},
	},
	async created() {
		this.fetchPage();
		// https://vueschool.io/lessons/reacting-to-param-changes?friend=vuerouter
		// using below metho because :key="$route.path" in App.vue <router-view /> does not add .router-link-active class
		this.$watch(() => this.$route.params, this.fetchPage);
	},
};
</script>

<style scoped>
section {
	margin-top: 100px;
}

.fade-enter-active,
.fade-leave-active {
	transition: opacity 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
	opacity: 0;
}

.slide-fade-enter-active,
.slide-fade-leave-active {
	transition: all 0.3s ease-out;
}
.slide-fade-enter-from,
.slide-fade-leave-to {
	transform: translateX(20px);
	opacity: 0;
}
</style>
