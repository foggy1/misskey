<template>
<div class="qkcjvfiv _section">
	<MkButton @click="create" primary class="add"><Fa :icon="faPlus"/> {{ $t('createList') }}</MkButton>

	<MkPagination :pagination="pagination" #default="{items}" class="lists _content" ref="list">
		<div class="list _panel" v-for="(list, i) in items" :key="list.id">
			<MkA :to="`/my/lists/${ list.id }`">{{ list.name }}</MkA>
		</div>
	</MkPagination>
</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { faListUl, faPlus } from '@fortawesome/free-solid-svg-icons';
import MkPagination from '@/components/ui/pagination.vue';
import MkButton from '@/components/ui/button.vue';
import * as os from '@/os';

export default defineComponent({
	components: {
		MkPagination,
		MkButton,
	},

	data() {
		return {
			INFO: {
				title: this.$t('manageLists'),
				icon: faListUl,
				action: {
					icon: faPlus,
					handler: this.create
				}
			},
			pagination: {
				endpoint: 'users/lists/list',
				limit: 10,
			},
			faListUl, faPlus
		};
	},

	methods: {
		async create() {
			const { canceled, result: name } = await os.dialog({
				title: this.$t('enterListName'),
				input: true
			});
			if (canceled) return;
			await os.api('users/lists/create', { name: name });
			this.$refs.list.reload();
			os.success();
		},
	}
});
</script>

<style lang="scss" scoped>
.qkcjvfiv {
	> .add {
		margin: 0 auto var(--margin) auto;
	}

	> .lists {
		> .list {
			display: flex;
			padding: 16px;
		}
	}
}
</style>
