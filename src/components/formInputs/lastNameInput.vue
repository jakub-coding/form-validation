<template>
	<div class="input">
		<div class="input--inner">
			<label for="last-name">Příjmení:</label>
			<input id="last-name" v-model="lastName" type="text">
		</div>
		<div class="input--error">
			<p v-if="showErrorMessage">{{errorMessage}}</p>
		</div>
	</div>
</template>

<script>
import _ from "lodash"
export default {
	name: "lastNameInput",
	data() {
		return {
			showErrorMessage: false,
			errorMessage: "",

			lastName: "",
		}
	},

	methods: {
		validateInput() {
			const validationPayload = {
				input: "lastName",
				isValid: false,
				value: this.lastName
			}

			if(this.lastName !== "") {
				this.showErrorMessage = false
				validationPayload.isValid = true

				this.$emit("input-validation", validationPayload)
			} else {
				this.showErrorMessage = true
				validationPayload.isValid = false

				this.errorMessage = "Pole Příjmení je povinné"
				this.$emit("input-validation", validationPayload)
			}
		}
	},

	watch: {
		lastName: _.debounce(function() {
			this.validateInput()
		}, 200)
	}
}
</script>

<style scoped>

</style>
