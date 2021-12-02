<template>
	<h2>Convert between IDA styled byte signatures and code styled byte signatures</h2>
	<p>{{result}}</p>
	<TextBox placeholder="IDA Style Signature" @submitted="convertIDASignature"/>
	<p>OR</p>
	<TextBox ref="codeSig" style="margin-bottom: 10px;" placeholder="Code Style Signature" @submitted="convertCodeSignature"/>
	<TextBox ref="codeMask" placeholder="Code Style Mask" @submitted="convertCodeSignature"/>
	<br>
	<iframe style="margin-top: 160px;" width="560" height="315" src="https://www.youtube.com/embed/LE36XH4hkN4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</template>

<script>
import TextBox from './components/TextBox.vue'

export default {
	name: 'App',
	components: {
		TextBox
	},
	data() {
		return {
			result: '',
			idaStyleUnknownByte: '?',
			codeStyleUnknownByte: '00'
		}
	},
	methods: {
		convertIDASignature(signature) {
			const bytes = signature.split(' ');

			if (bytes[bytes.length - 1] == '') { // If there is a space at the end of the signature, remove it.
				bytes.pop();
			}

			const codeSignature = [];
			const codeSignatureMask = [];

			for (const byte of bytes) {
				if (byte == this.idaStyleUnknownByte) {
					codeSignature.push(`\\x${this.codeStyleUnknownByte}`);
					codeSignatureMask.push('?');
				} else {
					codeSignature.push(`\\x${byte}`);
					codeSignatureMask.push('x');
				}
			}

			this.result = `${codeSignature.join('')} | ${codeSignatureMask.join('')}`;
		},

		convertCodeSignature() {
			const bytes = this.$refs.codeSig.$refs.textbox.value.split('\\x');
			const mask = this.$refs.codeMask.$refs.textbox.value.split('');

			bytes.shift(); // Remove first empty result.
			console.log(bytes, mask);

			const idaSignature = [];

			for (const [index, byte] of bytes.entries()) {
				if (mask[index] == '?') {
					idaSignature.push(this.idaStyleUnknownByte);
				} else {
					idaSignature.push(byte);
				}
			}

			this.result = idaSignature.join(' ');
		}
	}
}
</script>

<style>
body {
	background-color: rgb(25, 25, 25);
	color: rgb(255, 255, 255);
    font-family: 'Ubuntu', sans-serif;
}

#app {
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
}

h2 {
	margin-bottom: 30px;
}
</style>