<template>
	<img :src="dataurl" />
</template>

<script>
import blockies from 'ethereum-blockies-updated'
export default {
	name: 'EthImage',
	props: {
		opts: {
			type: Object,
			required: true,
		},
	},
	computed: {
		dataurl() {
			const opts = this.opts
			opts.seed = opts.seed.toLowerCase()
			const icon = blockies.create(opts)
			return icon.toDataURL()
		},
	},
}
</script>
