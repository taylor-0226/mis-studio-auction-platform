<template>
	<div :style="spinner_wrapper">
		<div class="vue-simple-spinner" :style="spinner_style"></div>
		<div
			v-if="message.length > 0"
			class="vue-simple-spinner-text"
			:style="text_style"
		>
			{{ message }}
		</div>
		<div v-if="showText" class="vue-simple-spinner-text" :style="text_style">
			Try connecting your wallet, this page requires information from the Ethereum
			blockchain.
		</div>
		{{ connection }}
	</div>
</template>

<script>
import { mapGetters } from 'vuex'
const isNumber = function (n) {
	return !isNaN(parseFloat(n)) && isFinite(n)
}
export default {
	props: {
		size: {
			type: String || Number,
			// either a number (pixel width/height) or 'tiny', 'small',
			// 'medium', 'large', 'huge', 'massive' for common sizes
			default: 32,
		},
		lineSize: {
			type: Number,
			default: 3,
		},
		lineBgColor: {
			type: String,
			default: '#eee',
		},
		lineFgColor: {
			type: String,
			default: 'var(--primary)', // match .blue color to Material Design's 'Blue 500' color
		},
		speed: {
			type: Number,
			default: 0.8,
		},
		spacing: {
			type: Number,
			default: 4,
		},
		message: {
			type: String,
			default: '',
		},
		fontSize: {
			type: Number,
			default: 13,
		},
		textFgColor: {
			type: String,
			default: '#555',
		},
	},
	data() {
		return {
			timer: '',
			showText: false,
		}
	},
	computed: {
		...mapGetters({
			coinbase: 'ethereum/coinbase',
		}),
		size_px() {
			switch (this.size) {
				case 'tiny':
					return 12
				case 'small':
					return 16
				case 'medium':
					return 32
				case 'large':
					return 48
				case 'big':
					return 64
				case 'huge':
					return 96
				case 'massive':
					return 128
			}
			return isNumber(this.size) ? this.size : 32
		},
		line_size_px() {
			switch (this.size) {
				case 'tiny':
					return 1
				case 'small':
					return 2
				case 'medium':
					return 3
				case 'large':
					return 3
				case 'big':
					return 4
				case 'huge':
					return 4
				case 'massive':
					return 5
			}
			return isNumber(this.lineSize) ? this.lineSize : 4
		},
		text_margin_top() {
			switch (this.size) {
				case 'tiny':
				case 'small':
				case 'medium':
				case 'large':
				case 'big':
				case 'huge':
				case 'massive':
					return Math.min(Math.max(Math.ceil(this.size_px / 8), 3), 12)
			}
			return isNumber(this.spacing) ? this.spacing : 4
		},
		text_font_size() {
			switch (this.size) {
				case 'tiny':
				case 'small':
				case 'medium':
				case 'large':
				case 'big':
				case 'huge':
				case 'massive':
					return Math.min(Math.max(Math.ceil(this.size_px * 0.4), 11), 32)
			}
			return isNumber(this.fontSize) ? this.fontSize : 13
		},
		spinner_wrapper() {
			return {
				margin: '60px 0',
			}
		},
		spinner_style() {
			return {
				margin: '0 auto',
				'border-radius': '100%',
				border: this.line_size_px + 'px solid ' + this.lineBgColor,
				'border-top': this.line_size_px + 'px solid ' + this.lineFgColor,
				width: this.size_px + 'px',
				height: this.size_px + 'px',
				animation: 'vue-simple-spinner-spin ' + this.speed + 's linear infinite',
			}
		},
		text_style() {
			return {
				'margin-top': this.text_margin_top + 'px',
				color: this.textFgColor,
				'font-size': this.text_font_size + 'px',
				'text-align': 'center',
			}
		},
		connection() {
			if (this.coinbase !== null) {
				clearTimeout(this.timer)
			}
			return null
		},
	},
	mounted() {
		this.timer = setTimeout(() => {
			this.showText = true
		}, 30000)
	},
	destroyed() {
		clearTimeout(this.timer)
	},
	methods: {
		action() {
			this.showText = false
		},
	},
}
</script>

<style>
.vue-simple-spinner {
	transition: all 0.3s linear;
}
@keyframes vue-simple-spinner-spin {
	0% {
		transform: rotate(0deg);
	}
	100% {
		transform: rotate(360deg);
	}
}
</style>
