<template>
  <div class="ticket">
    <div class="ticket-left">
      <div class="d-flex align-center justify-space-between row w-100">
        <div class="airlines d-flex align-center">
          <img
            v-if="ticket.validating_carrier"
            :src="`https://aviata.kz/static/airline-logos/80x80/${ticket.validating_carrier}.png`"
            width="20px"
            height="20px"
            alt=""
          />
          {{ findName(ticket.validating_carrier) }}
        </div>

        <div class="">
          <div
            class=""
            v-for="(itinerary, itinerary_index) in ticket.itineraries"
            :key="itinerary_index"
          >
            <div
              class="route d-flex align-center"
              v-for="(itemj, indexj) in itinerary"
              :key="indexj"
            >
              <div class="dates">
                <p>{{ getDate(itemj.dep_date) }}</p>
                <h2>{{ getTime(itemj.dep_date) }}</h2>
              </div>
              <div class="route-details">
                <div class="d-flex align-center justify-space-between">
                  <p class="city_code">{{ itemj.segments[0].origin_code }}</p>
                  <p class="route_time">
                    {{ totalTime(itemj.traveltime) }}
                  </p>
                  <p class="city_code">
                    {{ itemj.segments[itemj.segments.length - 1].origin_code }}
                  </p>
                </div>
                <div class="line">
                  <div
                    class="circle"
                    v-for="(segment, segment_index) in itemj.segments"
                    :key="segment_index"
                  ></div>
                  <div class="circle"></div>
                </div>
                <p
                  class="orange-color"
                  :class="{
                    'd-none': segment_index == itemj.segments.length - 1,
                  }"
                  v-for="(segment, segment_index) in itemj.segments"
                  :key="segment_index"
                >
                  через {{ segment.dest }}, {{ calculateFlightTime(segment) }}
                </p>
              </div>
              <div class="dates">
                <p>
                  {{ getDate(itemj.arr_date) }}
                  <span>{{ getTimezoneRange(itemj.segments) }}</span>
                </p>
                <h2>{{ getTime(itemj.arr_date) }}</h2>
              </div>
            </div>
          </div>
        </div>
        <div class=""></div>
      </div>
      <div class="details">
        <p class="blue-color">Детали перелета</p>
        <p class="blue-color">Условия тарифа</p>
        <p v-if="!ticket.refundable" class="notreturnable">
          <svg
            width="20"
            height="20"
            viewBox="0 0 20 20"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M4.62478 4.63247L2.18383 5.00014L2 3.77966L5.66142 3.22817L6.21291 6.88958L4.99244 7.07341L4.62478 4.63247Z"
              fill="#707276"
            />
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M5.50024 3.53116C3.45478 4.96007 2.1168 7.33198 2.1168 10.0164C2.1168 14.3825 5.65618 17.9218 10.0222 17.9218C14.3883 17.9218 17.9277 14.3825 17.9277 10.0164C17.9277 5.66297 14.4087 2.13144 10.0601 2.11105L10.035 3.11098C13.8429 3.11789 16.9277 6.2069 16.9277 10.0164C16.9277 13.8302 13.836 16.9218 10.0222 16.9218C6.20847 16.9218 3.1168 13.8302 3.1168 10.0164C3.1168 7.82671 4.13598 5.87507 5.72584 4.60995L5.50024 4.61576V3.99864V3.53116Z"
              fill="#707276"
            />
            <path
              d="M1.64918 16.7664L1.2933 17.1176L1.64451 17.4735L2.51148 18.352L2.86269 18.7078L3.21857 18.3566L18.2986 3.47419L18.6545 3.12298L18.3032 2.7671L17.4363 1.88863L17.0851 1.53275L16.7292 1.88397L1.64918 16.7664Z"
              fill="#707276"
              stroke="white"
            />
          </svg>
          невозвратный
        </p>
      </div>
    </div>
    <div class="ticket-right">
      <h2 class="">
        {{ ticket.price }}
        {{ ticket.currency == "KZT" ? " ₸" : ` ${ticket.currency}` }}
      </h2>
      <button class="green-btn">Выбрать</button>
      <p class="small">Цена за всех пассажиров</p>
      <div class="bag">
        Нет багажа
        <button @click="addBag" class="bag-btn">+ Добавить багаж</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    months: {
      1: "Января",
      2: "Февраля",
      3: "Марта",
      4: "Апреля",
      5: "Мая",
      6: "Июня",
      7: "Июля",
      8: "Августа",
      9: "Сентября",
      10: "Октября",
      11: "Ноября",
      12: "Декабря",
    },
    week_days: {
      1: "пн",
      2: "вт",
      3: "ср",
      4: "чт",
      5: "пт",
      6: "сб",
      0: "вс",
    },
  }),
  props: {
    ticket: Object,
    airlines: Object,
  },
  methods: {
    calculateFlightTime(segment) {
      let start = new Date(segment.dep_time_iso),
        end = new Date(segment.arr_time_iso),
        result = this.totalTime(Math.floor((end - start) / 1000));

      return result;
    },
    getTimezoneRange(segments) {
      let start = segments[0].dep_time_iso.substring(19, 22),
        end = segments[segments.length - 1].arr_time_iso.substring(19, 22);
      return `+${parseInt(start) - parseInt(end)}`;
    },
    totalTime(traveltime) {
      let hours = Math.floor(traveltime / 3600),
        minutes = Math.floor((traveltime % 3600) / 60);
      return `${hours} ч ${minutes} м`;
    },
    getDate(date) {
      let year = date.substring(0, 4),
        month = parseInt(date.split("/")[1]),
        day_of_week = new Date(date).getDay();

      month = this.months[month].substring(0, 3).toLowerCase();
      return `${year} ${month}, ${this.week_days[day_of_week]}`;
    },
    getTime(date) {
      return date.substring(10);
    },
    findName(code) {
      return this.airlines[code];
    },
    addBag() {
      console.log(this.ticket);
    },
  },
};
</script>

<style lang="scss" scoped>
.orange-color {
  color: #ff9900;
}
.blue-color {
  color: #7284e4;
}
.ticket {
  font-family: Open Sans;
  font-style: normal;
  font-weight: normal;
  font-size: 12px;
  line-height: 16px;
  color: #202123;

  display: grid;
  grid-template-columns: 1fr 240px;
  width: 100%;
  background: #ffffff;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.15);
  border-radius: 4px;
  margin-bottom: 12px;

  &-left {
    padding: 40px 43px 16px;
  }
  &-right {
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 12px 20px 18px;
    background: #f5f5f5;

    text-align: center;
    h2 {
      font-size: 24px;
      line-height: 28px;
    }
    .green-btn {
      max-width: unset;
      margin-top: 13px;
      height: 40px;
      font-weight: bold;
      font-size: 18px;
      line-height: 25px;
    }
    .small {
      margin-top: 8px;
      color: #707276;
      font-size: 12px;
      line-height: 16px;
    }
    .bag {
      margin-top: 12px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      font-size: 12px;
      line-height: 16px;
      &-btn {
        background: #eaf0fa;
        border-radius: 2px;
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 3px 8px;
        font-weight: 600;
        color: #5763b3;

        font-size: 12px;
        line-height: 16px;
      }
    }
  }
}
.airlines {
  img {
    margin-right: 7px;
  }
}
.route {
  margin: 20px 0px;
  .dates {
    span {
      vertical-align: middle;
      font-weight: 600;
      font-size: 10px;
      line-height: 14px;
      color: rgba(255, 55, 36, 0.8);
      border-radius: 4px;
    }
    h2 {
      font-weight: 600;
      font-size: 24px;
      line-height: 33px;
      color: #202123;
    }
  }
  .city_code {
    font-size: 10px;
    line-height: 12px;
    color: #b9b9b9;
  }
  .route_time {
    font-size: 12px;
    line-height: 18px;
  }
  &-details {
    min-width: 168px;
    margin: 0 30px;
  }
}
.details {
  margin-top: 46px;
  p {
    display: inline-block;
    padding-bottom: 2px;
    border-bottom: 1px dashed #7284e4;
    margin-right: 23px;
    margin-bottom: 10px;
  }
  .blue-color {
    cursor: pointer;
    transition: 0.22s;
    &:hover {
      opacity: 0.5;
      border-bottom: 1px dashed #7284e4;
    }
    :last-child {
      margin-right: 46px;
    }
  }
  .notreturnable {
    color: #707276;
    border: none;
    white-space: nowrap;
    svg {
      margin-right: 7px;
      margin-bottom: -6px;
    }
  }
}

.line {
  margin: 4px 0px 5px;
  width: 100%;
  height: 1px;
  background: #b9b9b9;
  display: flex;
  justify-content: space-between;
  .circle {
    width: 5px;
    height: 5px;
    border-radius: 50%;
    background: white;
    border: 1px solid #b9b9b9;
    z-index: 2;
    transform: translateY(-50%);
  }
}
</style>
