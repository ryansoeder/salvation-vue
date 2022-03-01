<template>
	<div id="nav">
        <router-link to="/">Home</router-link>
		<router-link v-for="item in menu" :key="item.ID" :to="this.getSlug(item.url)">{{ item.title }} </router-link>
	</div>
</template>

<script>
export default {
    name: 'TheNav',
    data() {
        return {
            menu: null,
        }
    },
    methods: {
        async fetchMenu() {
			const res = await fetch(
				`http://tattoo-salvation.local/wp-json/wp/v2/main_menu`
			);
			this.menu = await res.json();
            console.log(this.menu);
		},
        getSlug(url) {
            const urlArray = url.split('/');
            return urlArray[urlArray.length - 2];
        }
    },
    async created() {
        this.fetchMenu();
    }
};
</script>

<style scoped lang="scss">
#nav {
	padding: 30px;

	a {
		font-weight: bold;
		color: #2c3e50;
        margin-right: 20px;
        &:last-of-type {
            margin-right: unset;
        }

		&.router-link-active {
			color: #42b983;
		}
	}
}
</style>
