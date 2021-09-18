<template>
  <div class="container">
    <Search class="search" />

    <div class="home-grid">
      <div class="filter">
        <VFilter v-if="filters.length > 0" :filters="filters" />
      </div>
      <div class="tickets">
        <div class="tickets-item" v-for="(item, index) in tickets" :key="index">
          <Ticket :ticket="item" :airlines="airlines" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Search from "@/components/Search.vue";
import VFilter from "@/components/Filter.vue";
import Ticket from "@/components/Ticket.vue";
import results from "@/assets/test-task-aviata/results";

export default {
  name: "Home",
  data: () => ({
    results: results,
    tickets: [],
    filters: [],
    airlines: {},
  }),
  mounted() {
    this.setTickets();
    this.setFilters(this.results.airlines);
  },
  methods: {
    setTickets() {
      this.tickets = this.results.flights;
    },
    setFilters(airlines) {
      let array = Object.entries(airlines).map(([key, value]) => ({
        code: key,
        text: value,
      }));
      this.airlines = airlines;
      this.filters.push(
        {
          name: "tariffs",
          text: "Опции тарифа",
          items: [
            {
              text: "Только прямые",
            },
            {
              text: "Только с багажом",
            },
            {
              text: "Только возвратные",
            },
          ],
        },
        {
          name: "airlines",
          text: "Авиакомпании",
          items: array,
        }
      );
    },
  },
  components: {
    Search,
    VFilter,
    Ticket,
  },
};
</script>

<style lang="scss" scoped>
.search {
  margin-top: 22px;
}
.home-grid {
  display: grid;
  grid-template-columns: 240px 1fr;
  margin-top: 23px;
}
.tickets {
  margin-left: 20px;
}
</style>
