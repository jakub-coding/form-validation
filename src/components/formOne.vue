<template>
	<first-name-input @input-validation="inputValidation" ref="callFirstNameValidation"/>
	<last-name-input @input-validation="inputValidation" ref="callLastNameValidation"/>
	<email-input @input-validation="inputValidation" ref="callEmailValidation"/>
	<password-input @input-validation="inputValidation" ref="callPasswordValidation" />
</template>

<script>
import FirstNameInput from "@/components/formInputs/firstNameInput"
import LastNameInput from "@/components/formInputs/lastNameInput"
import EmailInput from "@/components/formInputs/emailInput"
import PasswordInput from "@/components/formInputs/passwordInput"
export default {
	name: "formOne",
	components: { PasswordInput, EmailInput, LastNameInput, FirstNameInput },
	data() {
		return {
			firstNameValid: false,
			lastNameValid: false,
			emailValid: false,
			passwordValid: false,

			dataPartOne: {
				valueFirstName: "",
				valueLastName: "",
				valueEmail: "",
				valuePassword: "",
			}
		}
	},

	methods: {
		inputValidation(data) {
			if(data.input === "firstName") {
				this.firstNameValid = data.isValid
				this.dataPartOne.valueFirstName = data.value
			}
			if(data.input === "lastName") {
				this.lastNameValid = data.isValid
				this.dataPartOne.valueLastName = data.value
			}
			if(data.input === "email") {
				this.emailValid = data.isValid
				this.dataPartOne.valueEmail = data.value
			}
			if(data.input === "password") {
				this.passwordValid = data.isValid
				this.dataPartOne.valuePassword = data.value
			}

			this.validateAllInputs()
		},

		validateAllInputs() {

			if(this.firstNameValid && this.lastNameValid && this.emailValid && this.passwordValid) {
				this.$emit("validation", true)
				this.$emit("outputOne", this.dataPartOne)
			} else {
				this.$emit("validation", false)
			}
		},


		validateInputs() {
			this.$refs.callFirstNameValidation.validateInput()
			this.$refs.callLastNameValidation.validateInput()
			this.$refs.callEmailValidation.validateInput()
			this.$refs.callPasswordValidation.validateInput()
		}
	},
}
</script>

<style scoped>

</style>
