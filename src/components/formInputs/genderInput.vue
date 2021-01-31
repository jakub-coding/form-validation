<template>
	<div class="input">
		<div class="input--inner">
			<label for="gender">Pohlaví</label>
			<select id="gender" v-model="gender" >
				<option value="">- Vyberte -</option>
				<option value="men">Muž</option>
				<option value="women">Žena</option>
			</select>
		</div>
		<div class="input--error">
			<p v-if="showErrorMessage">{{errorMessage}}</p>
		</div>
	</div>
</template>

<script>
import _ from "lodash"
export default {
	name: "genderInput",

	data() {
		return {
			showErrorMessage: false,
			errorMessage: "",

			gender: "",
		}
	},

	methods: {
		validateInput() {
			const validationPayload = {
				input: "gender",
				isValid: false,
				value: this.gender,
			}

			if(this.gender !== "") {
				this.showErrorMessage = false
				validationPayload.isValid = true

				this.$emit("input-validation", validationPayload)
			} else {
				this.showErrorMessage = true
				validationPayload.isValid = false
				this.errorMessage = "Pole Pohlaví je povinné"

				this.$emit("input-validation", validationPayload)
			}
		}
	},

	watch: {
		gender: _.debounce(function() {
			this.validateInput()
		}, 200)
	}
}
</script>

<style scoped>

</style>
