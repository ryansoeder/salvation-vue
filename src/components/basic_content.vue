<template>
	<section :class="block.acf_fc_layout.replaceAll('_', '-')">
		<div class="wrapper">
			<div class="row">
				<div class="col">
					<div class="content">
						<h1 class="title">{{ block.title }}</h1>
						<p class="content" v-html="block.content" />
					</div>
				</div>
			</div>
		</div>
	</section>
</template>

<script>
export default {
	name: 'BasicContent',
	props: {
		block: Object,
	},
};
</script>
