<template>
	<gender-input @input-validation="inputValidation" ref="callGenderValidation" />
	<birthday-input @input-validation="inputValidation" ref="callBirthdayValidation" />
	<marketing-input @input-validation="inputValidation" ref="callMarketingValidation"/>
	<confirmation-input @input-validation="inputValidation" ref="callConfirmationValidation"/>
</template>

<script>
import GenderInput from "@/components/formInputs/genderInput"
import BirthdayInput from "@/components/formInputs/birthdayInput"
import MarketingInput from "@/components/formInputs/marketingInput"
import ConfirmationInput from "@/components/formInputs/confirmationInput"
export default {
	name: "formTwo",
	components: { ConfirmationInput, MarketingInput, BirthdayInput, GenderInput },

	data() {
		return {
			genderValid: false,
			birthdayValid: false,
			marketingValid: false,
			confirmationValid: false,

			dataPartTwo: {
				valueGender: "",
				valueBirthday: "",
				valueMarketing: "",
				valueConfirmation: "",
			}
		}
	},

	methods: {
		inputValidation(data) {

			if(data.input === "gender") {
				this.genderValid = data.isValid
				this.dataPartTwo.valueGender = data.value
			}
			if(data.input === "birthday") {
				this.birthdayValid = data.isValid
				this.dataPartTwo.valueBirthday = data.value
			}
			if(data.input === "marketing") {
				this.marketingValid = data.isValid
				this.dataPartTwo.valueMarketing = data.value
			}
			if(data.input === "confirmation") {
				this.confirmationValid = data.isValid
				this.dataPartTwo.valueConfirmation = data.value
			}

			this.validateAllInputs()
		},

		validateAllInputs() {
			if(this.genderValid && this.birthdayValid && this.marketingValid && this.confirmationValid) {
				this.$emit("validation", true)
				this.$emit("outputTwo", this.dataPartTwo)
			} else {
				this.$emit("validation", false)
			}
		},


		validateInputs() {
			this.$refs.callGenderValidation.validateInput()
			this.$refs.callBirthdayValidation.validateInput()
			this.$refs.callMarketingValidation.validateInput()
			this.$refs.callConfirmationValidation.validateInput()
		}
	}
}
</script>

<style scoped>

</style>
