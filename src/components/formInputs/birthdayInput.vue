<template>
	<div class="input">
		<div class="input--inner">
			<label for="birthday">Datum narození</label>
			<input id="birthday" v-model="birthday" type="date">
		</div>
		<div class="input--error">
			<p v-if="showErrorMessage">{{errorMessage}}</p>
		</div>
	</div>
</template>

<script>
import _ from "lodash"
export default {
	name: "birthdayInput",

	data() {
		return {
			showErrorMessage: false,
			errorMessage: "",

			birthday: "",
		}
	},

	methods: {
		validateInput() {
			const validationPayload = {
				input: "birthday",
				isValid: false,
				value: this.birthday,
			}

			if(this.birthday !== "") {
				this.showErrorMessage = false
				validationPayload.isValid = true

				this.$emit("input-validation", validationPayload)
			} else {
				this.showErrorMessage = true
				validationPayload.isValid = false
				this.errorMessage = "Pole Datum narození je povinné"

				this.$emit("input-validation", validationPayload)
			}
		}
	},

	watch: {
		birthday: _.debounce(function() {
			this.validateInput()
		}, 200)
	}
}
</script>

<style scoped>

</style>
