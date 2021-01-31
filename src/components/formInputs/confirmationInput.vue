<template>
	<div class="input">
		<div class="input--inner__confirmation">
			<input id="user-confirm" v-model="confirm" type="checkbox">
			<label for="user-confirm">
				Souhlasím s obchodními podmínkami a beru na vědomí poučení o zpracování mých osobních údajů .
			</label>
		</div>
		<div class="input--error">
			<p v-if="showErrorMessage">{{ errorMessage }}</p>
		</div>
	</div>
</template>

<script>
export default {
	name: "confirmationInput",

	data () {
		return {
			showErrorMessage: false,
			errorMessage: "",

			confirm: false
		}
	},

	methods: {
		validateInput() {
			const validationPayload = {
				input: "confirmation",
				isValid: false,
				value: this.confirm
			}

			if(this.confirm) {
				this.showErrorMessage = false
				validationPayload.isValid = true

				this.$emit("input-validation", validationPayload)
			} else {
				this.showErrorMessage = true
				validationPayload.isValid = false
				this.errorMessage = "Nutný souhlas s obchodními podmínkami a zpracováním osobních údajů"

				this.$emit("input-validation", validationPayload)
			}
		}
	},

	watch: {
		confirm: function() {
			this.validateInput()
		}
	}
}
</script>

<style lang="scss" scoped>
	@import "src/assets/scss/app";

	.input {
		margin: 5rem 0 2rem 0;
	}

	.input--inner__confirmation {
		display: flex;
		justify-content: start;
		align-items: center;
		input {
			margin-right: 1rem;
		}

		label {
			font-family: $second-font;
			font-size: 1rem;
		}
	}
</style>
