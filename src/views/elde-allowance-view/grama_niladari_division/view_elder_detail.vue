<template>
	<div>
		<b-card no-body class="mb-4">
			<b-row>
				<b-colxx lg="6" md="12" class="mb-4">
					<div class="position-absolute card-top-buttons"></div>
					<single-lightbox
						:thumb="img"
						:large="img"
						class-name="card-img-top "
						class="m-4"
					/>
				</b-colxx>
				<b-colxx lg="6" md="12" class="mb-4">
					<div class="m-4">
						<p class="text-muted text-small mb-2">Full Name</p>
						<p class="mb-3">{{elder.name}}</p>

						<p class="text-muted text-small mb-2">Address</p>
						<p class="mb-3">{{elder.address}}</p>

						<p class="mb-3">
							<span class="text-muted text-small mb-2">Sex:</span>
							{{elder.sex}}
						</p>
						<p class="mb-3">
							<span class="text-muted text-small mb-2">Phone No:</span>
							{{elder.number}}
						</p>

						<p class="mb-3">
							<span class="text-muted text-small mb-2">Nic No:</span>
							{{elder.nic_id}}
						</p>
						<p class="mb-3">
							<span class="text-muted text-small mb-2">Email:</span>
							{{elder.email}}
						</p>

						<p class="mb-3">
							<span class="text-muted text-small mb-2">Date of Birth:</span>
							{{elder.birth_day }}
						</p>
						<p class="mb-3">
							<span class="text-muted text-small mb-2">Age:</span>
															 {{elder.age}}

						</p>
					</div>
				</b-colxx>
			</b-row>
			<b-card-body>
				<b-row>
					<b-colxx lg="6" md="12" class="mb-4">
						<p class="mb-3">
							<span class="text-muted text-small mb-2">Local Elders Commity Name:</span>
							{{elder.local_commity_elder_name}}
						</p>
						<p>
							<span class="text-muted text-small mb-2">Local Elders Commity Membership No:</span>
							{{elder.local_commity_elder_id}}
						</p>
						<p class="text-muted text-small mb-2">Nearest Post Office</p>
						<p class="mb-3">{{elder.near_post_office_id}}</p>

						<p class="text-muted text-small mb-2">Lives With Whome</p>
						<p class="mb-3">{{elder.lives_with_whome}}</p>

						<p class="text-muted text-small mb-2">Other Elder Nic</p>
						<p class="mb-3">{{elder.other_elders_nic}}</p>

						<p class="text-muted text-small mb-2">Samurdhi Number</p>
						<p class="mb-3">{{elder.samurdi_no}}</p>

						<p class="text-muted text-small mb-2">Public Aid Number</p>
						<p class="mb-3">{{elder.people_adi_no}}</p>
					</b-colxx>
					<b-colxx lg="6" md="12" class="mb-4">
						<p class="text-muted text-small mb-2">Other Details</p>
						<p class="mb-3">{{elder.other_name_and_description}}</p>

						<p class="text-muted text-small mb-2">Source Of Income</p>
						<p class="mb-3">{{elder.source_of_income}}</p>

						<p class="text-muted text-small mb-2">Monthly Income</p>
						<p class="mb-3">{{elder.income}}</p>
					</b-colxx>
				</b-row>
			</b-card-body>
		</b-card>
	</div>
</template>

<script>
import AppLayout from "../../../layouts/EAppLayout";
import SingleLightbox from "../../../containers/pages/SingleLightbox";
import axios from "axios";
import {bUrl} from '../../../constants/config'
import { validationMixin } from "vuelidate";
const { required, maxLength, minLength } = require("vuelidate/lib/validators");

export default {
	name: "view-application-and-verify",
	components: {
		AppLayout: AppLayout,
		"single-lightbox": SingleLightbox
	},
	data() {
		return {
			submit_div: true,
			elder: {},
			img: "",
			grama_comment: ""
		};
	},
	props: ["id"],
	mixins: [validationMixin],
	validations: {
		grama_comment: {
			required,
			maxLength: maxLength(256),
			minLength: minLength(10)
		}
	},
	async created() {
		axios({
			method: "get",
			url: "/elders/" + this.id
		}).then(result => {
			this.elder = result.data.data;
			this.elder.birth_day = this.elder.birth_day.split("T", 1)[0];
			console.log(new Date());
			console.log(result.data.data);
						 this.elder.age =  (new Date().getFullYear() -  new Date(this.elder.birth_day).getFullYear() );

			// this.aplications = result.data.data;
		});

		const body = {
				id:this.id,
				role_id:"10"
			}
			axios({
			method: "post",
			url: "/upload/getprofile"  ,
			data:body
		}).then(res => {
			console.log(res.data.data[0].profile);
			this.img = bUrl+res.data.data[0].profile;
		}).catch(cc => {
			console.log(cc);
			this.img = "/assets/img/profiles/def.png"
		});
	},
	methods: {
		onValitadeFormSubmit() {
			this.$v.$touch();
			console.log(this.$v.$invalid + " dis king ");
			if (!this.$v.$invalid) {
				const body = {
					gramaniladari_id: "2",
					gramaniladari_comment: this.grama_comment,
					correction: this.grama_comment,
					elder_id: this.id
				};
				axios({
					method: "patch",
					url: "/verifyelder/gramadisqualify",
					data: body
				})
					.then(res => {
						console.log("Disqualified res");
						console.log(res);
					})
					.catch(err => {
						console.log(err);
					});
				console.log(
					JSON.stringify({
						messsage: this.grama_comment
					})
				);
				this.submit_div = !this.submit_div;
			}
		},
		accept() {
			this.$v.$touch();
			console.log(this.$v.$invalid + "  ase cking ");
			if (!this.$v.$invalid) {
				const body = {
					gramaniladari_id: "2",
					gramaniladari_comment: this.grama_comment,
					elder_id: this.id
				};
				axios({
					method: "patch",
					url: "/verifyelder/gramaaccept",
					data: body
				})
					.then(res => {
						console.log("Accept res");
						console.log(res);
					})
					.catch(err => {
						console.log(err);
					});
				console.log(
					JSON.stringify({
						messsage: this.grama_comment
					})
				);
				this.submit_div = !this.submit_div;
			}
		}
	}
};
</script>
<style>
p {
	font-size: 1.4em;
}
</style>
