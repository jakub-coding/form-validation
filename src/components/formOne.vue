<template>
	<div class="input">
		<div class="input--inner">
			<label for="first-name">Jméno:</label>
			<input id="first-name" v-model="firstName" type="text">
			<p class="input__error">{{firstNameError}}</p>
		</div>
	</div>

	<div class="input">
		<div class="input--inner">
			<label for="last-name">Příjmení:</label>
			<input id="last-name" v-model="lastName" type="text">
			<p class="input__error">{{lastNameError}}</p>
		</div>
	</div>

	<div class="input">
		<div class="input--inner">
			<label for="email">E-mail:</label>
			<input v-model="email" id="email" type="email">
			<p class="input__error">{{emailError}}</p>
		</div>
	</div>

	<div class="input">
		<div class="input--inner">
			<label for="password">Heslo:</label>
			<input v-model="password" id="password" type="password">
			<p class="input__error">{{passwordError}}</p>
		</div>
	</div>
</template>

<script>
import _ from "lodash"
export default {
	name: "formOne",
	data() {
		return {
			firstNameValid: false,
			lastNameValid: false,
			emailValid: false,
			passwordValid: false,

			firstNameError: "",
			lastNameError: "",
			emailError: "",
			passwordError: "",

			firstName: "",
			lastName: "",
			email: "",
			password: "",
		}
	},

	methods: {
		checkValidation() {
			this.validateFirstName()
			this.validateLastName()
			this.validateEmail()
			this.validatePassword()

			this.checkIfValidate()
		},

		validateFirstName() {
			if (this.firstName) {
				this.firstNameValid = true
				this.firstNameError = ""
			} else {
				this.firstNameValid = false
				this.firstNameError = "Pole Jméno je povinné"
			}

		},

		validateLastName() {
			if(this.lastName) {
				this.lastNameValid = true
				this.lastNameError = ""
			} else {
				this.lastNameValid = false
				this.lastNameError = "Pole Příjmení je povinné"
			}
		},

		validateEmail() {
			const mailFormat = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/

			if(this.email) {
				if(this.email.match(mailFormat)){
					this.emailValid = true
					this.emailError = ""
				} else {
					this.emailValid = false
					this.emailError = "Email je ve špatném formátu"
				}
			} else {
				this.emailValid = false
				this.emailError = "Pole Email je povinné"
			}
		},

		validatePassword() {
			const passFormat = /(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}/

			if(this.password) {
				if(this.password.match(passFormat)) {
					this.passwordValid = true
					this.passwordError = ""
				} else {
					this.passwordValid = false
					this.passwordError = "Heslo musí obsahovat minimálně 8 znaků, velké a malé písmeno a číslici"
				}
			} else {
				this.passwordValid = false
				this.passwordError = "Pole Heslo je povinné"
			}
		},

		checkIfValidate() {
			const dataPayload = {
				isValidate: false,
				firstName: this.firstName,
				lastName: this.lastName,
				email: this.email,
				password: this.password,
			}

			if (this.firstNameValid && this.lastNameValid && this.emailValid && this.passwordValid) {
				dataPayload.isValidate = true
				this.$emit("outputOne", dataPayload)
			}
		},
	},

	watch: {
		firstName: _.debounce(function () {
			this.validateFirstName()
		}, 200),

		lastName: _.debounce(function () {
			this.validateLastName()
		}, 200),

		email: _.debounce(function () {
			this.validateEmail()
		}, 200),

		password: _.debounce(function () {
			this.validatePassword()
		}, 200)
	}

}
</script>

<style scoped>

</style>
