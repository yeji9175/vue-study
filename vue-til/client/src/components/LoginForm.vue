<template>
	<div class="contents">
		<div class="form-wrapper form-wrapper-sm">
			<form @submit.prevent="submitForm" class="form">
				<div>
					<label for="username">id:</label>
					<input id="username" type="text" v-model="username" />
					<p class="validation-text">
						<span class="warning" v-if="!isFormValid">
							Please enter an email address
						</span>
					</p>
				</div>
				<div>
					<label for="password">pw:</label>
					<input id="password" type="text" v-model="password" />
				</div>
				<button
					:disabled="!isFormValid"
					type="submit"
					class="btn"
					:class="isFormValid ? null : 'disabled'"
				>
					로그인
				</button>
			</form>
			<p class="log">{{ logMessage }}</p>
		</div>
	</div>
</template>

<script>
import { validateEmail } from '@/utils/validation';

export default {
	data() {
		return {
			// form
			username: '',
			password: '',
			// log
			logMessage: '',
		};
	},
	computed: {
		isFormValid() {
			return validateEmail(this.username) && this.password;
		},
	},
	methods: {
		async submitForm() {
			try {
				// NOTE: await을 해주지 않으면 로직 흐름이 안맞음
				await this.$store.dispatch('LOGIN', {
					username: this.username,
					password: this.password,
				});
				this.$router.push('/main');
			} catch (error) {
				this.logMessage = error.response.data;
				console.log(error.response);
			} finally {
				this.initForm();
			}
		},
		initForm() {
			this.username = '';
			this.password = '';
		},
	},
};
</script>

<style>
.btn {
	color: white;
}
</style>
