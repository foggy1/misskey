<template>
<div class="_section">
	<div class="_content">
		<XNotes ref="notes" :pagination="pagination" @before="before" @after="after"/>
	</div>
</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { faSearch } from '@fortawesome/free-solid-svg-icons';
import Progress from '@/scripts/loading';
import XNotes from '@/components/notes.vue';

export default defineComponent({
	components: {
		XNotes
	},

	data() {
		return {
			INFO: {
				title: this.$t('searchWith', { q: this.$route.query.q }),
				icon: faSearch
			},
			pagination: {
				endpoint: 'notes/search',
				limit: 10,
				params: () => ({
					query: this.$route.query.q,
				})
			},
		};
	},

	watch: {
		$route() {
			(this.$refs.notes as any).reload();
		}
	},

	methods: {
		before() {
			Progress.start();
		},

		after() {
			Progress.done();
		}
	}
});
</script>
