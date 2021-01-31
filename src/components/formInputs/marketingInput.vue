<template>
	<div class="input">
		<p>
			Chcete dostávat informace z hráčské kabiny a vědět jako první o akcích pro fanoušky a speciálních nabídkách
			fanshopu? Uděluji souhlas se zpracováním osobních údajů pro marketingové účely.
		</p>
		<div class="input--inner__radio">
			<div class="radio__item">
				<input id="marketing-true" v-model="marketing" :value="true" type="radio">
				<label for="marketing-true">Ano</label>
			</div>
			<div class="radio__item">
				<input id="marketing-false" v-model="marketing" :value="false" type="radio">
				<label for="marketing-true">Ne</label>
			</div>
		</div>
		<div class="input--error">
			<p v-if="showErrorMessage">{{ errorMessage }}</p>
		</div>
	</div>
</template>

<script>
export default {
	name: "marketingInput",

	data () {
		return {
			showErrorMessage: false,
			errorMessage: "",

			marketing: null
		}
	},

	methods: {
		validateInput() {
			const validationPayload = {
				input: "marketing",
				isValid: false,
				value: this.marketing,
			}

			if(this.marketing !== null) {
				this.showErrorMessage = false
				validationPayload.isValid = true

				this.$emit("input-validation", validationPayload)
			} else {
				this.showErrorMessage = true
				validationPayload.isValid = false
				this.errorMessage = "Vyberte jednu z možností"

				this.$emit("input-validation", validationPayload)
			}
		}
	},

	watch: {
		marketing: function() {
			this.validateInput()
		}
	}
}
</script>

<style lang="scss" scoped>
@import "src/assets/scss/app";
.input {
	margin-top: 3rem;
}

p {
	font-family: $second-font;
	font-size: .7rem;
	margin-bottom: 1rem;
}

.input--inner__radio {
	display: flex;
	justify-content: start;

	.radio__item {
		margin-right: 1rem;

		label {
			font-size: 1.2rem;
		}
	}
}
</style>
