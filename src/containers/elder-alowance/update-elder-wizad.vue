<template>
	<b-colxx xl="8" lg="12" style="margin:auto ">
		<b-card no-body>
			<b-card-body class="wizard wizard-default">
				<form-wizard :last-step-end="true" :elder="this.elder">
					<tab :name="$t('wizard.step-name-1')" desc="Update Elder details" :selected="true">
						<div class="wizard-basic-step">
							<b-form>
								<b-form-group>
									<elderformcon1 @changexx="changexx" />
								</b-form-group>
							</b-form>
						</div>
					</tab>
					<tab :name="$t('wizard.step-name-2')" desc="Elder Income details">
						<div class="wizard-basic-step">
							<b-form>
								<b-form-group>
									<elderformcon2 :elder="this.elder" @changexx="changexx" />
								</b-form-group>
							</b-form>
						</div>
					</tab>
				 
					<tab type="done">
						<div class="wizard-basic-step text-center">
							<h2 class="mb-2">{{$t('wizard.content-thanks')}}</h2>
							<p>{{$t('wizard.registered')}}</p>
						</div>
					</tab>
				</form-wizard>
			</b-card-body>
		</b-card>
	</b-colxx>
</template>

<script>
import FormWizard from "../../components/elders_component/elder_form/update-elder_wizad/updateElFormWizad";
import Tab from "../../components/elders_component/elder_form/update-elder_wizad/UpdateElTab";
import elderformcon1 from "./UpdateElderForrms/elderform-con-1";
import elderformcon2 from "./UpdateElderForrms/elderform-con-2.vue";
 

export default {
	components: {
		"form-wizard": FormWizard,
		tab: Tab,
		elderformcon1: elderformcon1,
		elderformcon2: elderformcon2,
		 
	},

	data() {
		return {
			elder: {
				name: "sahan"
			}
		};
	},
	methods: {
		fff() {
			console.log("helll");
		},
		changexx(el) {
			this.elder = el;
			console.log("ssss");
			console.log(this.elder);
		}
	}
};
</script>
