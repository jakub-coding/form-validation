<template>
	<div class="block">
		<div class="block__container">
			<h1 class="container__title">User Registration</h1>

			<form name="regForm" class="container__form" method="post" action="#">

				<h1 class="form__title">
					{{ ! formPart? "1" : "2" }}
					<span>/2</span>
					{{ ! formPart? "- Osobní údaje" : "- Další údaje"}}
				</h1>

				<div v-show="! formPart" class="form__part-one">
					<form-one ref="validateFirst" @output-one="formOneOutputHandler"/>
				</div>

				<div v-show="formPart" class="form__part-two">
					<form-two ref="validateSecond" @output-two="formTwoOutputHandler"/>
				</div>

				<button v-if="!formPart" @click="onNextClick" class="form__button--next" type="button">Pokračovat</button>

				<div v-if="formPart" class="form__buttons">
					<div class="buttons__label">
						<button @click="formPartSwitcher" class="form__button--back" type="button">Zpět</button>
					</div>
					<div class="buttons__label">
						<button  class="form__button--submit" @click.prevent="onSubmitClick" type="submit">Odeslat</button>
					</div>
				</div>

			</form>

		</div>
	</div>
</template>

<script lang="ts">
import { defineComponent } from "vue"
import FormOne from "@/components/formOne.vue"
import FormTwo from "@/components/formTwo.vue"
import axios from "axios"

export default defineComponent({
	name: "App",
	components: { FormTwo, FormOne },

	data() {
		return {
			formPart: false,
			partOneValid: false,
			partTwoValid: false,

			dataPayload: {
				email: "",
				password: "",
				firstName: "",
				lastName: "",
				birthday: "",
				gender: "",
				marketingConsent: false,
			}
		}
	},

	methods: {
		formPartSwitcher() {
			this.formPart = !this.formPart
		},

		// eslint-disable-next-line @typescript-eslint/ban-ts-ignore
		//@ts-ignore
		formOneOutputHandler(data) {
			if (data.isValidate) {

				this.dataPayload.firstName = data.firstName
				this.dataPayload.lastName = data.lastName
				this.dataPayload.email = data.email
				this.dataPayload.password = data.password

				this.formPartSwitcher()
			}
		},

		// eslint-disable-next-line @typescript-eslint/ban-ts-ignore
		//@ts-ignore
		formTwoOutputHandler(data) {
			if(data.isValidate) {
				this.dataPayload.gender = data.gender
				this.dataPayload.birthday = data.birthday
				this.dataPayload.marketingConsent = data.marketing

				this.postData()
			}
		},

		postData() {
			axios.post("http://localhost:8080", this.dataPayload).then(response => {
				console.log(response.data)
			}).catch(error => {
				console.error(error)
			})
		},

		onNextClick() {
			// eslint-disable-next-line @typescript-eslint/ban-ts-ignore
			//@ts-ignore
			this.$refs.validateFirst.checkValidation()
		},

		onSubmitClick() {
			// eslint-disable-next-line @typescript-eslint/ban-ts-ignore
			//@ts-ignore
			this.$refs.validateSecond.checkValidation()
		}

	}
})
</script>

<style lang="scss">
	@import "assets/scss/app";
</style>
