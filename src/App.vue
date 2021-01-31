<template>
	<div class="block">
		<div class="block__container">
			<h1 class="container__title">User Registration</h1>

			<form class="container__form" method="post" action="#">

				<h1 class="form__title">
					{{ ! formPart? "1" : "2" }}
					<span>/2</span>
					{{ ! formPart? "- Osobní údaje" : "- Další údaje"}}
				</h1>

				<div v-show="! formPart" class="form__part-one">
					<form-one ref="callValidationOne" @validation="partOneValidation" @output-one="outputDataHandleOne"/>
				</div>

				<div v-show="formPart" class="form__part-two">
					<form-two ref="callValidationTwo" @validation="partTwoValidation" @output-two="outputDataHandleTwo"/>
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
				marketingConsent: "",
			}
		}
	},

	methods: {
		formPartSwitcher() {
			this.formPart = !this.formPart
		},

		partOneValidation(data: boolean) {
			this.partOneValid = data
		},

		partTwoValidation(data: boolean) {
			this.partTwoValid = data
		},

		// eslint-disable-next-line @typescript-eslint/ban-ts-ignore
		//@ts-ignore
		outputDataHandleOne(data) {
			this.dataPayload.email = data.valueEmail
			this.dataPayload.password = data.valuePassword
			this.dataPayload.firstName = data.valueFirstName
			this.dataPayload.lastName = data.valueLastName
		},

		// eslint-disable-next-line @typescript-eslint/ban-ts-ignore
		//@ts-ignore
		outputDataHandleTwo(data) {
			this.dataPayload.birthday = data.valueBirthday
			this.dataPayload.gender = data.valueGender
			this.dataPayload.marketingConsent = data.valueMarketing
		},

		onNextClick() {
			if(this.partOneValid) {
				this.formPartSwitcher()
			} else {
				// eslint-disable-next-line @typescript-eslint/ban-ts-ignore
				//@ts-ignore
				this.$refs.callValidationOne.validateInputs()
			}
		},

		onSubmitClick() {
			if(this.partTwoValid) {
				axios.post("127.0.0.1", this.dataPayload).then(response => {
					console.log(response.data)
				}).catch(error => {
					console.error(error)
				})
			} else {
				// eslint-disable-next-line @typescript-eslint/ban-ts-ignore
				//@ts-ignore
				this.$refs.callValidationTwo.validateInputs()
			}
		}

	}
})
</script>

<style lang="scss">
	@import "assets/scss/app";
</style>
