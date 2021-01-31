<template>
	<div class="input">
		<div class="input--inner">
			<label for="gender">Pohlaví</label>
			<select id="gender" v-model="gender" >
				<option value="">- Vyberte -</option>
				<option value="men">Muž</option>
				<option value="women">Žena</option>
			</select>
			<p class="input__error">{{genderError}}</p>
		</div>
	</div>

	<div class="input">
		<div class="input--inner">
			<label for="birthday">Datum narození</label>
			<input id="birthday" v-model="birthday" type="date">
			<p class="input__error">{{birthdayError}}</p>
		</div>
	</div>

	<div class="input">
		<p>
			Chcete dostávat informace z hráčské kabiny a vědět jako první o akcích pro fanoušky a speciálních nabídkách
			fanshopu? Uděluji souhlas se zpracováním osobních údajů pro marketingové účely.
		</p>
		<div class="input--inner__radio">

			<div class="radio__item--field">
				<div class="radio__item">
					<input id="marketing-true" v-model="marketing" :value="true" type="radio">
					<label for="marketing-true">Ano</label>
				</div>
				<div class="radio__item">
					<input id="marketing-false" v-model="marketing" :value="false" type="radio">
					<label for="marketing-false">Ne</label>
				</div>
			</div>

			<p class="input__error">{{marketingError}}</p>
		</div>
	</div>

	<div class="input">
		<div class="input--inner__confirmation">
			<input id="user-confirm" v-model="confirmation" type="checkbox">
			<label for="user-confirm">
				Souhlasím s obchodními podmínkami a beru na vědomí poučení o zpracování mých osobních údajů .
			</label>
			<p class="input__error">{{confirmationError}}</p>
		</div>
	</div>
</template>

<script>
export default {
	name: "formTwo",

	data() {
		return {
			genderValid: false,
			birthdayValid: false,
			marketingValid: false,
			confirmationValid: false,

			genderError: "",
			birthdayError: "",
			marketingError: "",
			confirmationError: "",

			gender: "",
			birthday: "",
			marketing: "",
			confirmation: false,
		}
	},

	methods: {
		checkValidation() {
			this.validateGender()
			this.validateBirthday()
			this.validateMarketing()
			this.validateConfirmation()

			this.checkIfValidate()
		},

		validateGender() {
			if (this.gender) {
				this.genderValid = true
				this.genderError = ""
			} else {
				this.genderValid = false
				this.genderError = "Pole Pohlaví je povinné"
			}
		},

		validateBirthday() {
			if (this.birthday) {
				this.birthdayValid = true
				this.birthdayError = ""
			} else {
				this.birthdayValid = false
				this.birthdayError = "Pole Datum narození je povinné"
			}
		},

		validateMarketing() {
			if(this.marketing !== ""){
				this.marketingValid = true
				this.marketingError = ""
			} else {
				this.marketingValid = false
				this.marketingError = "Vyberte jednu z možností"
			}
		},

		validateConfirmation() {
			if(this.confirmation) {
				this.confirmationValid = true
				this.confirmationError = ""
			} else {
				this.confirmationValid = false
				this.confirmationError = "nutný souhlas s obchodními podmínkami a zpracováním osobních údajů"
			}
		},

		checkIfValidate() {
			const dataPayload = {
				isValidate: false,
				gender: this.gender,
				birthday: this.birthday,
				marketing: this.marketing,
			}

			if (this.genderValid && this.birthdayValid && this.marketingValid && this.confirmationValid) {
				dataPayload.isValidate = true
				this.$emit("outputTwo", dataPayload)
			}
		},
	},

	watch: {
		gender: function () {
			this.validateGender()
		},

		birthday: function () {
			this.validateBirthday()
		},

		marketing: function () {
			this.validateMarketing()
		},

		confirmation: function () {
			this.validateConfirmation()
		}
	}
}
</script>

<style scoped>

</style>
