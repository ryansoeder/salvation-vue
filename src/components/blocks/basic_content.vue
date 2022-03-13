<template>
	<section :class="block.acf_fc_layout.replaceAll('_', '-')">
		<div class="wrapper">
			<div class="row">
				<div class="col">
					<div class="content">
						<h1 class="title" v-if="block.title">{{ block.title }}</h1>
						<p class="content" v-html="block.content" v-if="block.content" />
					</div>
				</div>
			</div>
		</div>
	</section>
</template>

<script>
import basicContentStyles from '@/styles/basic-content.scss'
export default {
	name: 'BasicContent',
	props: {
		block: Object,
	},
};
</script>
