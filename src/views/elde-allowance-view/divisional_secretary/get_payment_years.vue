<template>
	<AppLayout>
		<b-colxx lg="12" md="12" class="m-lg-4 text-center" style="mrgin-top:50px;">
			<h1>Payment By Years</h1>
		</b-colxx>
		<b-button variant="primary" class="mt-4" @click="exportPDF">Print Report</b-button>
		<br />
		<br />
		<b-row>
			<list-with-user-item
				v-for="(item, index) in history"
				:data="item"
				:detail-path="'/division/get-payment-by-months/'+item.year"
				:key="index"
			/>
		</b-row>
	</AppLayout>
</template>

<script>
import AppLayout from "../../../layouts/EAppLayout";
import ListWithUserItem from "../../../components/elders_component/PayYears";
import axios from "axios";
import jsPDF from "jspdf";
import "jspdf-autotable";

export default {
	name: "list-elders",
	components: {
		AppLayout: AppLayout,
		"list-with-user-item": ListWithUserItem
	},
	data() {
		return {
			history: []
		};
	},
	async beforeCreate() {
		axios.get("/paymentdivoff/years").then(result => {
			console.log(result.data.data[0]);
			this.history = result.data.data;
		});
	},
	methods: {
		exportPDF() {
			var vm = this;
			var columns = [
				{ title: "Year", dataKey: "year" },
				{ title: "Total Year Payments", dataKey: "year_tot" },
				{ title: "Year To Post Office", dataKey: "year_to_post" },
				{ title: "Year To Fund", dataKey: "year_to_fund" },
				{ title: "Elder Payment Count", dataKey: "pay_count" },
				{ title: "Paid To Elders", dataKey: "elder_got" },
				{ title: "Again Return to Div", dataKey: "not_recive" }
			];
			var doc = new jsPDF("p", "pt");
			doc.setFontSize("15");
			doc.text("Divisional Payment By Years", 150, 120);
			doc.text("Over 70 Monthly Allowance ", 170, 90);
			// doc.text("Post Office :", 40, 120);
			// doc.text(" " + this.post_off + " " + this.p_name, 120, 120);
			// doc.text(" Head  ", 120, 160);
			doc.autoTable(columns, vm.history, {
				margin: { top: 200 }
			});
			// doc.setFontSize("12");
			// doc.text(
			// 	"I certify that above allowance contained on ................. sheat and totalling  ....................................\nonly Rupees are authorized  in Registrer of Allowance and are due for payment",
			// 	40,
			// 	600
			// );

			// doc.text("Prepared By", 80, 640);
			// doc.text("Checked By", 320, 640);
			// doc.text("2020.03", 80, 680);
			// doc.text("Divisional Secretory", 320, 680);
			// doc.text(
			// 	"I certify that above allwance totalling Rupees ............................................................have been paid to \nthe person named or to their authorized Guardian whose authorities are atached and that allwance \ntotalling Rs...................................... have not been paid   ",
			// 	40,
			// 	720
			// );
			// doc.text("Date", 80, 780);
			// doc.text("Postmaster", 320, 780);
			doc.save("pay year" + ".pdf");
		}
	}
};
</script>
