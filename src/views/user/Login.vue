<template>
	<b-row class="h-100">
		<b-colxx xxs="12" md="10" class="mx-auto my-auto">
			<b-card class="auth-card" no-body>
				<div class="position-relative image-side">
					<p class="h2 mb-5">Welcome To Elders Allowance Management System for Gampaha Divisional Secretary.</p>
					<h3 class="white mt-5 font-weight-bolder">
						Please use your credentials to login
						<br />If you are not a member,
						<router-link tag="a" to="/user/register" class="white">
							<br />
							<br />
							<I>please register click Here</I>
						</router-link>.
					</h3>
				</div>
				<div class="form-side">
					<router-link tag="a" to="/">
						<span class="logo-single" />
					</router-link>
					<h6 class="mb-4">{{ $t('user.login-title')}}</h6>

					<b-form @submit.prevent="formSubmit" class="av-tooltip tooltip-label-right">
						<b-form-group :label="$t('user.username')"  >
							<b-form-input type="text" v-model="$v.form.email.$model" :state="!$v.form.email.$error" />
							<b-form-invalid-feedback v-if="!$v.form.email.required">Please enter your username</b-form-invalid-feedback> 
						</b-form-group>

						<b-form-group :label="$t('user.password')"  >
							<b-form-input
								type="password"
								v-model="$v.form.password.$model"
								:state="!$v.form.password.$error"
							/>
							<b-form-invalid-feedback v-if="!$v.form.password.required">Please enter your password</b-form-invalid-feedback>
							<b-form-invalid-feedback
								v-else-if="
                  !$v.form.password.minLength || !$v.form.password.maxLength
                "
							>
								Your password must be between 4 and 16
								characters
							</b-form-invalid-feedback>
						</b-form-group>
						<div class="d-flex justify-content-between align-items-center">
							<router-link tag="a" to="">
								<!-- {{ /user/forgot-password
								$t("user.forgot-password-question")
								}} -->
							</router-link>
							<b-button
								type="submit"
								variant="primary"
								size="lg"
								:disabled="processing"
								:class="{
                  'btn-multiple-state btn-shadow': true,
                  'show-spinner': processing,
                  'show-success': !processing && loginError === false,
                  'show-fail': !processing && loginError
                }"
							>
								<span class="spinner d-inline-block">
									<span class="bounce1"></span>
									<span class="bounce2"></span>
									<span class="bounce3"></span>
								</span>
								<span class="icon success">
									<i class="simple-icon-check"></i>
								</span>
								<span class="icon fail">
									<i class="simple-icon-exclamation"></i>
								</span>
								<span class="label">{{ $t("user.login-button") }}</span>
							</b-button>
						</div>
					</b-form>
				</div>
			</b-card>
		</b-colxx>
	</b-row>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import { validationMixin } from "vuelidate";
import { adminRoot, elderRoot, dofficerRoot, divisionalOffHeadRoot ,sysAdminRoot } from "../../constants/config";
import { UserRole } from "../../utils/auth.roles";

const {
	required,
	maxLength,
	minLength,
	email
} = require("vuelidate/lib/validators");

export default {
	data() {
		return {
			form: {
				email: "",
				password: ""
			}
		};
	},
	mixins: [validationMixin],
	validations: {
		form: {
			password: {
				required,
				maxLength: maxLength(16),
				minLength: minLength(4)
			},
			email: {
				required,
				minLength: minLength(4)
			}
		}
	},
	computed: {
		...mapGetters(["currentUser", "processing", "loginError"])
	},
	methods: {
		...mapActions(["login"]),
		formSubmit() {
			this.$v.$touch();

			console.log(this.form.email + " " + this.form.password);

			this.$v.form.$touch();
			if (!this.$v.form.$anyError) {
			this.login({
				email: this.form.email,
				password: this.form.password
			});
			}
		}
	},
	watch: {
		currentUser(val) {
			
			if (val && val.role == UserRole.DivisionalOfficers ) {
					this.$router.push(dofficerRoot);
			}
			else if (val && val.role == UserRole.DivisionalOffOfficer ) {
					this.$router.push(dofficerRoot);
			}
			else if (val && val.role == UserRole.DivisionalOffHead) {
					this.$router.push(divisionalOffHeadRoot);
			}
			else if (val && val.role == UserRole.SystemAdmin) {
					this.$router.push("/sysadmin/dashboards/");
			}
			else if (val && val.role == UserRole.PostOfficers) {
					this.$router.push("/post/post-officer-dashboard");
			}
			else if (val && val.role == UserRole.GramaNiladariOffices) {
					this.$router.push("/grama/gramaniladari-dashboard");
			}
			else if (val && val.uid) {
				setTimeout(() => {
					this.$router.push("/elder/dashboards/default");
				}, 2);
			}
		},
		loginError(val) {
			if (val != null) {
				this.$notify("error", "Login Error", val, {
					duration: 3000,
					permanent: false
				});
			}
		}
	},
		created() {
		if(localStorage.jwt && !localStorage.jwt==null ){
			var obj = JSON.parse(localStorage.user)
			if ( obj.role == UserRole.DivisionalOfficers ) {
					this.$router.push(dofficerRoot);
			}
			else if (obj.role == UserRole.DivisionalOffHead) {
					this.$router.push(divisionalOffHeadRoot);
			}
			else {
				setTimeout(() => {
					this.$router.push("/elder/dashboards/default");
				}, 2);
			}
		}
	}
};
</script>
