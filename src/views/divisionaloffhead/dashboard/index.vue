<template>
        <user-card-elder></user-card-elder>
</template>

<script>
import { mapGetters, mapActions } from "vuex";

import UserCardElder from "../../../containers/elder-alowance/divisionaloffhead/UserCard";
export default {

  components: {
   "user-card-elder": UserCardElder,
  },
  computed: {
  

	},
  methods: {
    
  },
  mounted() {
  }
};
</script>
