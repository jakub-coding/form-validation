<template>
	<div class="input">
		<div class="input--inner">
			<label for="email">E-mail:</label>
			<input v-model="email" id="email" type="email">
		</div>
		<div class="input--error">
			<p v-if="showErrorMessage">{{errorMessage}}</p>
		</div>
	</div>
</template>

<script>
import _ from "lodash"

export default {
	name: "emailInput",

	data() {
		return {
			showErrorMessage: false,
			errorMessage: "",

			email: "",
		}
	},

	methods: {
		validateInput() {
			const mailFormat = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/

			const validationPayload = {
				input: "email",
				isValid: false,
				value: this.email,
			}

			if(this.email !== "") {
				if(this.email.match(mailFormat)) {
					this.showErrorMessage = false
					validationPayload.isValid = true

					this.$emit("input-validation", validationPayload)
				} else {
					this.showErrorMessage = true
					validationPayload.isValid = false
					this.errorMessage = "Zadaný e-mail je ve špatném formátu"
					this.$emit("input-validation", validationPayload)
				}
			} else {
				this.showErrorMessage = true
				validationPayload.isValid = false
				this.errorMessage = "Pole E-mail je povinné"

				this.$emit("input-validation", validationPayload)
			}
		}
	},

	watch: {
		email: _.debounce(function() {
			this.validateInput()
		}, 200)
	}
}
</script>

<style scoped>

</style>
