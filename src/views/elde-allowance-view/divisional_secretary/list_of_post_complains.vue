<template>
	<AppLayout>
		<div>
			<b-overlay :show="show" spinner-variant="primary" spinner-type="grow" spinner-small rounded="sm">
				
				<datatable-heading
					title="The List Of Post Offices Complain"
					:selectAll="selectAll"
					:isSelectedAll="isSelectedAll"
					:isAnyItemSelected="isAnyItemSelected"
					:keymap="keymap"
					:changePageSize="changePageSize"
					:searchChange="searchChange"
					:from="from"
					:to="to"
					:total="total"
					:perPage="perPage"
				></datatable-heading>
				<b-row>
					<b-colxx xxs="12">
						<vuetable
							ref="vuetable"
							class="table-divided order-with-arrow"
							:http-fetch="getData"
							:api-url="apiBase"
							:query-params="makeQueryParams"
							:per-page="perPage"
							:reactive-api-url="true"
							:fields="fields"
							pagination-path
							:row-class="onRowClass"
							@vuetable:pagination-data="onPaginationData"
							@vuetable:row-clicked="rowClicked"
							@vuetable:cell-rightclicked="rightClicked"
							@vuetable:loading="show=true"
							@vuetable:load-success="show=false"
						>
							<template slot="actions1" slot-scope="props">
							<b-button
								class="mb-1"
								@click="() =>showw(props)"
								@click.prevent="accept"								
								variant="outline-primary"
							>View</b-button>
						</template>
						</vuetable>
						<vuetable-pagination-bootstrap
							class="mt-4"
							ref="pagination"
							@vuetable-pagination:change-page="onChangePage"
						/>
					</b-colxx>
				</b-row>

				<v-contextmenu ref="contextmenu">
					<v-contextmenu-item @click="onContextMenuAction('copy')">
						<i class="simple-icon-docs" />
						<span>Copy</span>
					</v-contextmenu-item>
					<v-contextmenu-item @click="onContextMenuAction('move-to-archive')">
						<i class="simple-icon-drawer" />
						<span>Move to archive</span>
					</v-contextmenu-item>
					<v-contextmenu-item @click="onContextMenuAction('delete')">
						<i class="simple-icon-trash" />
						<span>Delete</span>
					</v-contextmenu-item>
				</v-contextmenu>
			</b-overlay>
		</div>
	</AppLayout>
</template>

<script>
import axios from "axios";
import AppLayout from "../../../layouts/EAppLayout";
import Vuetable from "vuetable-2/src/components/Vuetable";
import VuetablePaginationBootstrap from "../../../components/Common/VuetablePaginationBootstrap";
import { bUrl } from "../../../constants/config";
import DatatableHeading from "../../../containers/datatable/DatatableHeading";

export default {
	props: ["title"],
	components: {
		name: "post-officer-elder-payment-authenticate-verify",
		AppLayout: AppLayout,
		vuetable: Vuetable,
		"vuetable-pagination-bootstrap": VuetablePaginationBootstrap,
		"datatable-heading": DatatableHeading
	},
	data() {
		return {
			clickedid: null,
			show: true,
			apiBase: bUrl + "/elders",
			isLoad: false,
			sort: "",
			page: 1,
			perPage: 8,
			search: "",
			from: 0,
			to: 0,
			total: 0,
			lastPage: 0,
			items: [],
			selectedItems: [],

			fields: [
				{
					name: "elder_id",
					sortField: "elder_id",
					title: "elder_id",
					titleClass: "",
					dataClass: "list-item-heading",
					width: "20%"
				},
				{
					name: "ename",
					sortField: "ename",
					title: "Name",
					titleClass: "",
					dataClass: "text-muted",
					width: "20%"
				},
				{
					name: "pname",
					sortField: "pname",
					title: "Post Office",
					titleClass: "",
					dataClass: "text-muted",
					width: "20%"
				},
				{
					name: "complain",
					sortField: "complain",
					title: "Complain",
					titleClass: "",
					dataClass: "text-muted",
					width: "25%"
				},
				{
					name: "__slot:actions1",
					title: "",
					titleClass: "center aligned text-right",
					dataClass: "center aligned text-right",
					width: "30%"
				}
			]
		};
	},

	methods: {
		getData() {
			return axios.get("http://localhost:3000/api/deadcomplain/postcomplains/");
		},
		showw(pp) {
			this.clickedid = pp.rowData.elder_id;
		},
		accept() {
         
				axios({
					method: "patch",
					url: "http://localhost:3000/api/deadcomplain/comp/"+this.clickedid,
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
						messsage: this.clickedid
					})
				);
		},
		makeQueryParams(sortOrder, currentPage, perPage) {
			this.selectedItems = [];
			return sortOrder[0]
				? {
						sort: sortOrder[0]
							? sortOrder[0].field + "|" + sortOrder[0].direction
							: "",
						page: currentPage,
						per_page: this.perPage,
						search: this.search
				  }
				: {
						page: currentPage,
						per_page: this.perPage,
						search: this.search
				  };
		},
		onRowClass(dataItem, index) {
			if (this.selectedItems.includes(dataItem.elder_id)) {
				return "selected";
			}
			return "";
		},

		rowClicked(dataItem, event) {
			const itemId = dataItem.elder_id;
			if (event.shiftKey && this.selectedItems.length > 0) {
				let itemsForToggle = this.items;
				var start = this.getIndex(itemId, itemsForToggle, "elder_id");
				var end = this.getIndex(
					this.selectedItems[this.selectedItems.length - 1],
					itemsForToggle,
					"elder_id"
				);
				itemsForToggle = itemsForToggle.slice(
					Math.min(start, end),
					Math.max(start, end) + 1
				);
				this.selectedItems.push(
					...itemsForToggle.map(item => {
						return item.elder_id;
					})
				);
				this.selectedItems = [...new Set(this.selectedItems)];
			} else {
				if (this.selectedItems.includes(itemId)) {
					this.selectedItems = this.selectedItems.filter(x => x !== itemId);
				} else this.selectedItems.push(itemId);
			}
		},
		rightClicked(dataItem, field, event) {
			event.preventDefault();
			if (!this.selectedItems.includes(dataItem.elder_id)) {
				this.selectedItems = [dataItem.elder_id];
			}
			this.$refs.contextmenu.show({ top: event.pageY, left: event.pageX });
		},
		onPaginationData(paginationData) {
			this.from = paginationData.from;
			this.to = paginationData.to;
			this.total = paginationData.total;
			this.lastPage = paginationData.last_page;
			this.items = paginationData.data;
			this.$refs.pagination.setPaginationData(paginationData);
		},
		onChangePage(page) {
			this.$refs.vuetable.changePage(page);
		},

		changePageSize(perPage) {
			this.perPage = perPage;
			this.$refs.vuetable.refresh();
		},

		searchChange(val) {
			this.search = val;
			this.$refs.vuetable.refresh();
		},

		selectAll(isToggle) {
			if (this.selectedItems.length >= this.items.length) {
				if (isToggle) this.selectedItems = [];
			} else {
				this.selectedItems = this.items.map(x => x.elder_id);
			}
		},
		keymap(event) {
			switch (event.srcKey) {
				case "select":
					this.selectAll(false);
					break;
				case "undo":
					this.selectedItems = [];
					break;
			}
		},
		getIndex(value, arr, prop) {
			for (var i = 0; i < arr.length; i++) {
				if (arr[i][prop] === value) {
					return i;
				}
			}
			return -1;
		},

		onContextMenuAction(action) {
			console.log(
				"context menu item clicked - " + action + ": ",
				this.selectedItems
			);
		}
	},
	computed: {
		isSelectedAll() {
			return this.selectedItems.length >= this.items.length;
		},
		isAnyItemSelected() {
			return (
				this.selectedItems.length > 0 &&
				this.selectedItems.length < this.items.length
			);
		}
	}
};
</script>