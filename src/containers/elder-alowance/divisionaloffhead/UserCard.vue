<template>
  <b-row>
    <b-colxx>
      <!-- <h5 class="mb-4 card-title">{{ $t('cards.user-card') }}</h5> -->
      <b-row>
        <b-colxx md="12" sm="12" lg="12" xxs="12">
          <b-card class="mb-4 text-center">
            <router-link to="?">
              <img
                :src="currentUser.img"
                alt="Card image cap"
                class="img-thumbnail list-thumbnail rounded-circle border-0 mb-4"
              />
              <h6 class="mb-1 card-subtitle">{{ currentUser.title }}</h6>
              <p class="text-muted text-small mb-4">Divisional Secretary</p>
            </router-link>
            <router-link to="/user/changepass" tag="b-button" size="sm" variant="outline-primary" >{{ $t('user.changepass') }}</router-link>
          </b-card>
        </b-colxx>
      </b-row>
    </b-colxx>
  </b-row>
</template>
<script>

import { mapGetters } from "vuex";

export default {
  	computed: {
		  ...mapGetters({
			currentUser: "currentUser",
			menuType: "getMenuType",
			menuClickCount: "getMenuClickCount",
			selectedMenuHasSubItems: "getSelectedMenuHasSubItems"
		})
	},

};
</script>
