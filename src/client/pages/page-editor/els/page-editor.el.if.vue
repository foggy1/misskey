<template>
<XContainer @remove="() => $emit('remove')" :draggable="true">
	<template #header><Fa :icon="faQuestion"/> {{ $t('_pages.blocks.if') }}</template>
	<template #func>
		<button @click="add()" class="_button">
			<Fa :icon="faPlus"/>
		</button>
	</template>

	<section class="romcojzs">
		<MkSelect v-model:value="value.var">
			<template #label>{{ $t('_pages.blocks._if.variable') }}</template>
			<option v-for="v in hpml.getVarsByType('boolean')" :value="v.name">{{ v.name }}</option>
			<optgroup :label="$t('_pages.script.pageVariables')">
				<option v-for="v in hpml.getPageVarsByType('boolean')" :value="v">{{ v }}</option>
			</optgroup>
			<optgroup :label="$t('_pages.script.enviromentVariables')">
				<option v-for="v in hpml.getEnvVarsByType('boolean')" :value="v">{{ v }}</option>
			</optgroup>
		</MkSelect>

		<XBlocks class="children" v-model:value="value.children" :hpml="hpml"/>
	</section>
</XContainer>
</template>

<script lang="ts">
import { defineComponent, defineAsyncComponent } from 'vue';
import { v4 as uuid } from 'uuid';
import { faPlus, faQuestion } from '@fortawesome/free-solid-svg-icons';
import XContainer from '../page-editor.container.vue';
import MkSelect from '@/components/ui/select.vue';
import * as os from '@/os';

export default defineComponent({
	components: {
		XContainer, MkSelect,
		XBlocks: defineAsyncComponent(() => import('../page-editor.blocks.vue')),
	},

	inject: ['getPageBlockList'],

	props: {
		value: {
			required: true
		},
		hpml: {
			required: true,
		},
	},

	data() {
		return {
			faPlus, faQuestion
		};
	},

	created() {
		if (this.value.children == null) this.value.children = [];
		if (this.value.var === undefined) this.value.var = null;
	},

	methods: {
		async add() {
			const { canceled, result: type } = await os.dialog({
				type: null,
				title: this.$t('_pages.chooseBlock'),
				select: {
					groupedItems: this.getPageBlockList()
				},
				showCancelButton: true
			});
			if (canceled) return;

			const id = uuid();
			this.value.children.push({ id, type });
		},
	}
});
</script>

<style lang="scss" scoped>
.romcojzs {
	padding: 0 16px 16px 16px;
}
</style>
