<template>
	<div class="input">
		<div class="input--inner">
			<label for="password">Heslo:</label>
			<input v-model="password" id="password" type="password">
		</div>
		<div class="input--error">
			<p v-if="showErrorMessage">{{errorMessage}}</p>
		</div>
	</div>
</template>

<script>
import _ from "lodash"

export default {
	name: "passwordInput",

	data() {
		return {
			showErrorMessage: false,
			errorMessage: "",

			password: "",
		}
	},

	methods: {
		validateInput() {
			const passFormat = /(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}/

			const validationPayload = {
				input: "password",
				isValid: false,
				value: this.password,
			}

			if(this.password !== "") {
				if(this.password.match(passFormat)) {
					this.showErrorMessage = false
					validationPayload.isValid = true

					this.$emit("input-validation", validationPayload)
				} else {
					this.showErrorMessage = true
					validationPayload.isValid = false
					this.errorMessage = "Heslo musí obsahovat minimálně 8 znaků, velké a malé písmeno a číslici"
					this.$emit("input-validation", validationPayload)
				}
			} else {
				this.showErrorMessage = true
				validationPayload.isValid = false
				this.errorMessage = "Pole Heslo je povinné"

				this.$emit("input-validation", validationPayload)
			}
		}
	},

	watch: {
		password: _.debounce(function() {
			this.validateInput()
		}, 200)
	}
}
</script>

<style scoped>

</style>
