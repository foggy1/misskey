<template>
<XModalWindow ref="dialog"
	:width="366"
	:height="500"
	@close="$refs.dialog.close()"
	@closed="$emit('closed')"
>
	<template #header>{{ $t('signup') }}</template>

	<div class="_section">
		<XSignup :auto-set="autoSet" @signup="onSignup"/>
	</div>
</XModalWindow>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import XModalWindow from '@/components/ui/modal-window.vue';
import XSignup from './signup.vue';

export default defineComponent({
	components: {
		XSignup,
		XModalWindow,
	},

	props: {
		autoSet: {
			type: Boolean,
			required: false,
			default: false,
		}
	},

	emits: ['done', 'closed'],

	methods: {
		onSignup(res) {
			this.$emit('done', res);
			this.$refs.dialog.close();
		}
	}
});
</script>
