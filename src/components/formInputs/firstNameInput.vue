<template>
	<div class="input">
		<div class="input--inner">
			<label for="first-name">Jméno:</label>
			<input id="first-name" v-model="firstName" type="text">
		</div>
		<div class="input--error">
			<p v-if="showErrorMessage">{{errorMessage}}</p>
		</div>
	</div>
</template>

<script>
import _ from "lodash"
export default {

	name: "firstNameInput",

	data() {
		return {
			showErrorMessage: false,
			errorMessage: "",

			firstName: "",
		}
	},

	methods: {
		validateInput() {
			const validationPayload = {
				input: "firstName",
				isValid: false,
				value: this.firstName,
			}

			if(this.firstName !== "") {
				this.showErrorMessage = false
				validationPayload.isValid = true

				this.$emit("input-validation", validationPayload)
			} else {
				this.showErrorMessage = true
				validationPayload.isValid = false
				this.errorMessage = "Pole Jméno je povinné"

				this.$emit("input-validation", validationPayload)
			}
		}
	},

	watch: {
		firstName: _.debounce(function() {
			this.validateInput()
		}, 200)
	}
}
</script>

<style scoped>

</style>
