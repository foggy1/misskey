<template>
<div class="nsbbhtug" v-if="hasDisconnected && $store.state.serverDisconnectedBehavior === 'quiet'" @click="resetDisconnected">
	<div>{{ $t('disconnectedFromServer') }}</div>
	<div class="command">
		<button class="_textButton" @click="reload">{{ $t('reload') }}</button>
		<button class="_textButton">{{ $t('doNothing') }}</button>
	</div>
</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import * as os from '@/os';

export default defineComponent({
	data() {
		return {
			hasDisconnected: false,
		}
	},
	computed: {
		stream() {
			return os.stream;
		},
	},
	created() {
		os.stream.on('_disconnected_', this.onDisconnected);
	},
	beforeUnmount() {
		os.stream.off('_disconnected_', this.onDisconnected);
	},
	methods: {
		onDisconnected() {
			this.hasDisconnected = true;
		},
		resetDisconnected() {
			this.hasDisconnected = false;
		},
		reload() {
			location.reload();
		},
	}
});
</script>

<style lang="scss" scoped>
.nsbbhtug {
	position: fixed;
	z-index: 16385;
	bottom: 8px;
	right: 8px;
	margin: 0;
	padding: 6px 12px;
	font-size: 0.9em;
	color: #fff;
	background: #000;
	opacity: 0.8;
	border-radius: 4px;
	max-width: 320px;

	> .command {
		display: flex;
		justify-content: space-around;

		> button {
			padding: 0.7em;
		}
	}
}
</style>
