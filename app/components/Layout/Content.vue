<template>
	<div class="content">
		<fade-transition :duration="200" mode="out-in">
			<!-- your content here -->
			<nuxt></nuxt>
		</fade-transition>
	</div>
</template>
<script>
import { FadeTransition } from 'vue2-transitions'
export default {
	components: {
		FadeTransition,
	},
}
</script>
<style></style>
