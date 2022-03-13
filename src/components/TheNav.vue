<template>
	<ul class="main-menu">
		<template v-for="item in menu" :key="item.ID">
			<li v-if="item.menu_item_parent == 0">
				<router-link v-if="internalLink(item.url)" :to="this.getSlug(item.url)">{{
					item.title
				}}</router-link>
				<a v-else :href="item.url" target="_blank">{{ item.title }}</a>
				<ul class="sub-menu" :v-if="hasSubItems(item.ID)">
					<template v-for="subItem in menu" :key="subItem.ID">
						<li v-if="subItem.menu_item_parent == item.ID">
							<router-link
								v-if="internalLink(subItem.url)"
								:to="this.getSlug(subItem.url)"
								>{{ subItem.title }}</router-link
							>
							<a v-else :href="subItem.url" target="_blank">{{
								subItem.title
							}}</a>
						</li>
					</template>
				</ul>
			</li>
		</template>
	</ul>
</template>

<script>
export default {
	name: 'TheNav',
	data() {
		return {
			menu: null,
		};
	},
	methods: {
		async fetchMenu() {
			const res = await fetch(
				`http://tattoo-salvation.local/wp-json/wp/v2/main_menu`
			);
			this.menu = await res.json();
			console.log(this.menu);
		},
		internalLink(url) {
            const newRL = new URL(url);
			if (newRL.hostname === 'tattoo-salvation.local') {
				return true;
			} else {
				return false;
			}
		},
		getSlug(url) {
			const newRL = new URL(url);
			return newRL.pathname;
		},
		hasSubItems(parentID) {
			let subItemFound = false; // kick off found flag
			this.menu.forEach((item) => {
				// loop through menu items
				if (item.menu_item_parent == parentID) {
					// if an item's parent ID matches the ID passed
					subItemFound = true; // flag is true
					return subItemFound; // return early
				} else {
					subItemFound = false; // otherwise, flag is false and keep looping
				}
			});
			return subItemFound;
		},
	},
	async created() {
		this.fetchMenu();
	},
};
</script>

<style scoped lang="scss">
.main-menu {
	display: flex;
	justify-content: center;
	li {
		margin: 0 10px;
		&:hover {
			.sub-menu {
				opacity: 1;
				visibility: visible;
			}
		}
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
}
.sub-menu {
	opacity: 0;
	visibility: hidden;
	transition: 0.3s ease-in-out;
	width: 100%;
	text-align: left;

	li {
		margin: 10px 0;
		width: max-content;
	}
}
.main-menu,
.sub-menu {
	list-style-type: none;
	margin: 0;
	padding: 0;
	li:before {
		content: none;
	}
}
</style>
