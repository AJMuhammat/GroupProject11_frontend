<template>
	<b-row>
		<b-colxx xxs="12">
			<div class="text-center">
				<h1>{{$t('elder.form')}}</h1>
			</div>

			<b-card class="mb-4">
				<!-- //there was an emit click method ss -->
			
				<b-form class="av-tooltip tooltip-label-right">

					
						<b-form-group :label="$t('user.username')">
							<b-form-input
								type="text"
								v-model="$v.elder.uname.$model"
								:state="!$v.elder.uname.$error"
							/>
							<b-form-invalid-feedback v-if="!$v.elder.uname.required">{{$t('user.e-uname')}}</b-form-invalid-feedback>
						</b-form-group>

						<b-form-group :label="$t('user.password')">
							<b-form-input
								type="text"
								v-model="$v.elder.pword.$model"
								:state="!$v.elder.pword.$error"
							/>
							<b-form-invalid-feedback v-if="!$v.elder.pword.required">{{$t('user.e-pass')}}</b-form-invalid-feedback>
							<b-form-invalid-feedback
								v-else-if="!$v.elder.pword.minLength"
							>{{$t('user.v-pass')}}</b-form-invalid-feedback>
						</b-form-group>

					<b-form-group :label="$t('elder.name')">
						<b-form-input type="text" v-model="$v.elder.name.$model" :state="!$v.elder.name.$error" />
						<b-form-invalid-feedback v-if="!$v.elder.name.required">{{$t('form.e-name')}}</b-form-invalid-feedback>
						<b-form-invalid-feedback
							v-else-if="!$v.elder.name.minLength || !$v.elder.maxLength"
						>{{$t('form.v-name')}}</b-form-invalid-feedback>
					</b-form-group>

					<b-form-group :label="$t('form.address')">
						<b-form-textarea
							type="text"
							v-model="$v.elder.address.$model"
							:state="!$v.elder.address.$error"
						/>
						<b-form-invalid-feedback v-if="!$v.elder.address.required">{{$t('form.e-address')}}</b-form-invalid-feedback>
						<b-form-invalid-feedback
							v-else-if="!$v.elder.address.minLength || !$v.elder.address.maxLength"
						>{{$t('form.v-address')}}</b-form-invalid-feedback>
					</b-form-group>

	 				<b-form-group :label="$t('form.photo')">
						<b-input-group :label="$t('input-groups.upload')" class="mb-3">
              				  <b-form-file   @change="onFileChange"  :placeholder="$t('input-groups.choose-file')"></b-form-file>
            			</b-input-group>
	 				</b-form-group>

					<b-form-group :label="$t('form.nic')">
						<b-form-input type="text" v-model="$v.elder.nic_no.$model" :state="!$v.elder.nic_no.$error" />
						<b-form-invalid-feedback v-if="!$v.elder.nic_no.required">{{$t('form.e-nic')}}</b-form-invalid-feedback>
						<b-form-invalid-feedback v-if="!$v.elder.nic_no.validnic">{{$t('form.e-valid')}}</b-form-invalid-feedback>
						<b-form-invalid-feedback
							v-else-if="!$v.elder.nic_no.minLength || !$v.elder.nic_no.maxLength"
						>{{$t('form.v-nic')}}</b-form-invalid-feedback>
					</b-form-group>

					<b-form-group :label="$t('form.email')" class="error-l-100">
						<b-form-input type="text" v-model="$v.elder.email.$model" :state="!$v.elder.email.$error" />
						<b-form-invalid-feedback v-if="!$v.elder.email.required">{{$t('form.e-mail')}}</b-form-invalid-feedback>
						<b-form-invalid-feedback v-else-if="!$v.elder.email.email">{{$t('form.v-mail')}}</b-form-invalid-feedback>
					</b-form-group>

					<b-form-group :label="$t('form.sex')">
						<b-form-radio
							v-model="$v.elder.sex.$model"
							:state="!$v.elder.sex.$error"
							value="male"
						>{{$t('form.male')}}</b-form-radio>
						<b-form-radio
							v-model="$v.elder.sex.$model"
							:state="!$v.elder.sex.$error"
							value="female"
						>{{$t('form.female')}}</b-form-radio>
					</b-form-group>

					<b-form-group :label="$t('form.number')" class="error-l-100">
						<b-form-input
							placeholder="033XXXXXXX"
							type="text"
							v-model="$v.elder.phone_no.$model"
							:state="!$v.elder.phone_no.$error"
						/>
						<b-form-invalid-feedback v-if="!$v.elder.phone_no.required">{{$t('form.e-number')}}</b-form-invalid-feedback>
						<b-form-invalid-feedback v-else-if="!$v.elder.phone_no.numeric">{{$t('form.n-number')}}</b-form-invalid-feedback>
						<b-form-invalid-feedback v-else-if="!$v.elder.phone_no.validphone">{{$t('form.vaid-no')}}</b-form-invalid-feedback>
						<b-form-invalid-feedback
							v-else-if="!$v.elder.phone_no.minLength || !$v.elder.phone_no.maxLength"
						>{{$t('form.v-number')}}</b-form-invalid-feedback>
					</b-form-group>

					<b-form-group :label="$t('form.district')">
						<b-form-select
							v-model="$v.elder.district.$model"
							:options="district_option"
							:state="!$v.elder.district.$error"
						></b-form-select>
						<b-form-invalid-feedback v-if="!$v.elder.district.required">{{$t('form.e-district')}}</b-form-invalid-feedback>
					</b-form-group>

					<b-form-group :label="$t('form.divisional')">
						<b-form-select
							v-model="$v.elder.divisional_off.$model"
							:options="divisional_off_option"
							:state="!$v.elder.divisional_off.$error"
						></b-form-select>
						<b-form-invalid-feedback v-if="!$v.elder.divisional_off.required">{{$t('form.e-divisional')}}</b-form-invalid-feedback>
					</b-form-group>
					<b-form-group :label="$t('form.gramaniladari-division')" class="error-l-125">
						<b-form-select
							v-model="$v.elder.grama_niladari_div.$model"
							:options="grama_niladari_div_option"
							:state="!$v.elder.grama_niladari_div.$error"
						></b-form-select>
						<b-form-invalid-feedback v-if="!$v.elder.grama_niladari_div.required">{{$t('form.e-grama')}}</b-form-invalid-feedback>
					</b-form-group>

					<b-form-group :label="$t('form.post')" class="error-l-100">
						<b-form-select
							v-model="$v.elder.nearest_post_office.$model"
							:options="post_off_option"
							:state="!$v.elder.nearest_post_office.$error"
						></b-form-select>
						<b-form-invalid-feedback v-if="!$v.elder.nearest_post_office.required">{{$t('form.e-post')}}</b-form-invalid-feedback>
					</b-form-group>

					<b-form-group :label="$t('form.birth')" class="error-l-125">
						<b-colxx xxs="12" xl="6" class="mb-4">
							<b-card>
								<b-form>
									<b-row class="mb-0">
										<b-colxx xxs="12">
											<b-form-group>
												<datepicker
													v-model="$v.elder.birth_day.$model"
													:state="!$v.elder.birth_day.$error"
													:inline="true"
													:bootstrap-styling="true"
													class="embeded"
												></datepicker>
												<b-form-invalid-feedback v-if="!$v.elder.birth_day.required">Please enter Date of Birth</b-form-invalid-feedback>
											</b-form-group>
										</b-colxx>
									</b-row>
								</b-form>
							</b-card>
						</b-colxx>
					</b-form-group>
				</b-form>
			</b-card>
		</b-colxx>
	</b-row>
</template>

<script>
import { validationMixin } from "vuelidate";
const {
	required,
	maxLength,
	minLength,
	alpha,
	email,
	sameAs,
	numeric,
	maxValue,
	minValue,
	helpers
} = require("vuelidate/lib/validators");

const upperCase = helpers.regex("upperCase", /^[A-Z]*$/);
const validnic =  helpers.regex("upperCase", /^(?:19|20)?\d{2}(?:[0-35-8]\d\d(?<!(?:000|500|36[7-9]|3[7-9]\d|86[7-9]|8[7-9]\d)))\d{4}(?:[vVxX])$/);
const validphone = helpers.regex("upperCase", /^(?:0|94|\+94|0094)?(?:(11|21|23|24|25|26|27|31|32|33|34|35|36|37|38|41|45|47|51|52|54|55|57|63|65|66|67|81|91)(0|2|3|4|5|7|9)|7(0|1|2|5|6|7|8)\d)\d{6}$/);
import Datepicker from "vuejs-datepicker";
import axios from "axios";
export default {
	components: {
		datepicker: Datepicker
	},
	data() {
		return {
			 file:null,
			elder: {
				uname:"",
				pword:"",
				name: "",
				address: "",
				img:"",
				nic_no: "",
				email: "",
				sex: "male",
				phone_no: "",
				district: null,
				divisional_off: null,
				grama_niladari_div: null,
				nearest_post_office: null,
				birth_day: "1997-11-07T16:41:00.000Z"
			},
			district_option: [
				{
					value: null,
					text: "Select an District/කරුණාකර දිස්ත්‍රික්කය තෝරන්න",
					disabled: true
				}
			],
			divisional_off_option: [
				{
					value: null,
					text:
						"Select an Division Secretary Office/කරුණාකර ප්‍රාදේශීය ලේකම් කාර්යාලය තෝරන්න",
					disabled: true
				}
			],
			grama_niladari_div_option: [
				{
					value: null,
					text:
						"Select Grama Niladari Division/කරුණාකර ග්‍රාම නිළධාරී කොඨ්ඨාෂ‍ය තෝරන්න",
					disabled: true
				}
			],
			post_off_option: [
				{
					value: null,
					text:
						"Select Nearest Post Office/කරුණාකර ළඟම ඇති තැපැල් කාර්යාලය තෝරන්න",
					disabled: true
				}
			]
		};
	},
	mixins: [validationMixin],
	validations: {
		elder: {
			uname:{
				required,
			},
			pword:{
			required,
				minLength: minLength(10),
			},
			name: {
				required,
				maxLength: maxLength(32),
				minLength: minLength(2)
			},
			address: {
				required,
				maxLength: maxLength(256),
				minLength: minLength(10)
			},
			nic_no: {
				required,
				validnic,
				maxLength: maxLength(10),
				minLength: minLength(10)
			},
			email: {
				required,
				email
			},
			sex: {
				required
			},
			phone_no: {
				required,
				numeric,
				validphone,
				maxLength: maxLength(10),
				minLength: minLength(10)
			},
			district: {
				required
			},
			divisional_off: {
				required
			},
			grama_niladari_div: {
				required
			},
			nearest_post_office: {
				required
			},
			birth_day: {
				required
			}
		}
	},
	created() {
		axios
			.get("/district/selectbox")
			.then(res => {
				console.log(res);
				this.district_option = [...this.district_option, ...res.data.data];
			})
			.catch(err => {
				console.log(err);
			});

		axios
			.get("/divisionaloffice/selectbox")
			.then(res => {
				console.log(res);
				this.divisional_off_option = [
					...this.divisional_off_option,
					...res.data.data
				];
			})
			.catch(err => {
				console.log(err);
			});
		axios
			.get("/gramadivision/selectbox")
			.then(res => {
				console.log(res);
				this.grama_niladari_div_option = [
					...this.grama_niladari_div_option,
					...res.data.data
				];
			})
			.catch(err => {
				console.log(err);
			});
		axios
			.get("/postoffice/selectbox")
			.then(res => {
				console.log(res);
				this.post_off_option = [...this.post_off_option, ...res.data.data];
			})
			.catch(err => {
				console.log(err);
			});
	},
	methods: {
		onValitadeFormSubmit() {
			this.$v.$touch();

			console.log(
				JSON.stringify({
					elder: this.elder
				})
			);
			if (!this.$v.$invalid) {
				return {
					valid: this.$v.$invalid,
					elder: this.elder
				};
			} else {
				return {
					valid: this.$v.$invalid
				};
			}
		},
		ss() {
			console.log("now");
			var el = {
				name: "ben"
			};
			this.$emit("changexx", el);
			console.log("end");
		},
		onFileChange(e){
			   const selectedFile = e.target.files[0]; // accessing file
				  this.file = selectedFile;
				  console.log(this.file);
				  const formData = new FormData();
      				formData.append("file", this.file);  // appending file

     // sending file to the backend
      axios
        .post("/upload/profile", formData)
        .then(res => {
		  console.log(res.data.path);
		  this.elder.img=res.data.path;

        })
        .catch(err => {
          console.log(err);
        });
		}
	},
	watch: {
		name: function() {
			console.log("change");
		}
	}
};
</script>
