<template>
  <div class="">
    <div class="box">
      <form @submit.prevent="submitSearch">
        <div class="d-flex">
          <h2
            class="type pointer"
            @click="type = 'twoway'"
            :class="{ 'type-active': type == 'twoway' }"
          >
            ТУДА И ОБРАТНО
          </h2>
          <div class="line"></div>
          <h2
            class="type pointer"
            @click="type = 'oneway'"
            :class="{ 'type-active': type == 'oneway' }"
          >
            В ОДНУ СТОРОНУ
          </h2>
        </div>
        <div class="search-bar d-flex align-center justify-space-between row">
          <input
            v-model="searchForm.from_location"
            placeholder="Алматы, Казахстан"
            type="text"
          />
          <svg
            width="24"
            height="20"
            viewBox="0 0 24 20"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
          >
            <rect
              x="0.306091"
              width="23.2654"
              height="20"
              fill="url(#pattern0)"
            />
            <defs>
              <pattern
                id="pattern0"
                patternContentUnits="objectBoundingBox"
                width="1"
                height="1"
              >
                <use
                  xlink:href="#image0"
                  transform="translate(0 -0.0816341) scale(0.03125 0.0363521)"
                />
              </pattern>
              <image
                id="image0"
                width="32"
                height="32"
                xlink:href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAQAAADZc7J/AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAAmJLR0QAAKqNIzIAAAAJcEhZcwAADdcAAA3XAUIom3gAAAAHdElNRQfiChMIEi41ddxdAAABH0lEQVRIx72SPU7DQBCF345zhQQfIYGkp3WSA5AiLQcAUeKCnsg4HdfgCkm2SIGUlPnjCBYgbuA1RSx71yiRZizxui2+T7NvBvjvRA+Tpv0mLo5XmtsKpoDecEDPVjAFYZIG2NsKxW9h2koXuMLGDJ6+RAJXIRIAk6a3yLrYmIGKMpnimGzJLPFvan/Bk+DTFjS64hLdNbIneL6ALnG2IPaVxmWJV0459qP78wIzRsfGnQli32jcDr9n69OC2Wr4Y8ISt9YY+0ajjQP1w4TzKXLwfRrw8HyCHN95/cdPHg6oejhAR1xtTSDBARW941oCVkqUp0Ejo9F2j4P1haJEocJeo0iRX6JcUT1ltqLYQphQgA/06EZSZTHFy10NXJZf/XWKdZHZ1AkAAAAldEVYdGRhdGU6Y3JlYXRlADIwMTgtMTAtMTlUMDg6MTg6NDYrMDI6MDA5STgGAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDE4LTEwLTE5VDA4OjE4OjQ2KzAyOjAwSBSAugAAABl0RVh0U29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuPBoAAAAASUVORK5CYII="
              />
            </defs>
          </svg>

          <input
            v-model="searchForm.to_location"
            placeholder="Астана, Казахстан"
            type="text"
          />
          <input
            v-model="searchForm.to_date"
            placeholder="08.11.2018"
            type="text"
          />
          <input
            v-model="searchForm.from_date"
            placeholder="16.11.2018"
            type="text"
          />
          <input
            v-model="searchForm.tariff"
            placeholder="2 пассажира"
            type="text"
          />

          <button class="green-btn" type="submit">Найти</button>
        </div>
      </form>
    </div>
    <div class="d-flex justify-end additional-btns">
      <button type="button" class="pointer">
        <svg
          width="20"
          height="20"
          viewBox="0 0 20 20"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <g clip-path="url(#clip0)">
            <path
              d="M15.6751 10.4907V7.5473C15.6894 5.30323 14.1524 3.81482 12.8318 3.22214C12.4591 3.05524 12.0668 2.9312 11.6667 2.83662V2.63633C11.6667 1.73406 10.9191 1 10.0001 1C9.08106 1 8.33339 1.73406 8.33339 2.63633V2.86738C7.97372 2.96 7.62038 3.07323 7.28471 3.22509C5.82537 3.88551 4.34669 5.47734 4.33336 7.54534V10.4907C4.33336 12.5813 3.58135 14.3751 2.09834 15.8222C2.01001 15.9086 1.97801 16.0359 2.01534 16.1528C2.05267 16.2693 2.15368 16.356 2.27668 16.377L5.44037 16.9133C6.03704 17.0145 6.57871 17.0953 7.08838 17.1614C7.67005 18.2813 8.81239 19 10.0611 19C11.3101 19 12.4528 18.2807 13.0341 17.1604C13.5414 17.0947 14.0804 17.0141 14.6741 16.9133L17.8375 16.377C17.9605 16.3563 18.0611 16.2696 18.0988 16.1528C18.1365 16.0363 18.1045 15.9086 18.0158 15.8222C16.4625 14.3067 15.6751 12.5129 15.6751 10.4907ZM9.00006 2.63633C9.00006 2.09503 9.44873 1.65453 10.0001 1.65453C10.5514 1.65453 11.0001 2.09503 11.0001 2.63633V2.6995C10.3357 2.60721 9.65873 2.61244 9.00006 2.71586V2.63633ZM12.2018 17.258C11.6954 17.9328 10.9057 18.3451 10.0607 18.3451C9.21606 18.3451 8.42672 17.9328 7.92039 17.2586C8.05105 17.2717 8.17805 17.2825 8.30472 17.293C8.36706 17.2982 8.43106 17.3048 8.49272 17.3097C8.53839 17.3133 8.58239 17.3156 8.62739 17.3185C9.59606 17.3879 10.4884 17.3882 11.4561 17.3205C11.5127 17.3165 11.5681 17.3136 11.6254 17.3093C11.6967 17.3038 11.7707 17.2966 11.8434 17.29C11.9577 17.2802 12.0714 17.2707 12.1888 17.2589C12.1934 17.2586 12.1974 17.2583 12.2018 17.258Z"
              fill="#FF9900"
            />
          </g>
          <defs>
            <clipPath id="clip0">
              <rect width="20" height="20" fill="white" />
            </clipPath>
          </defs>
        </svg>
        Отслеживать изменения цен
      </button>
      <button type="button" class="pointer">
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
            d="M13.9768 7.27985H18V16H13.9768V7.27985ZM7.9884 15.9837V9.43922H12.0116V15.9837H7.9884ZM2 16V4H6.0232V16H2Z"
            fill="#FF9900"
          />
        </svg>
        Посмотреть цены на соседние даты (+- 3дня)
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    type: "oneway",
    searchForm: {},
  }),
};
</script>

<style lang="scss" scoped>
.box {
  padding: 16px 18px;
  background: #fff;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.25);
  border-radius: 8px;
}
.line {
  border-right: 1px solid #cccccc;
  margin-right: 14px;
  margin-left: 14px;
}
.type {
  width: 143px;
  padding-bottom: 2px;
  transition: 0.22s;
  border-bottom: 2px solid transparent;
  color: #888888;
  &:hover,
  &-active {
    color: #000000;
    border-bottom: 2px solid #55bb06;
  }
}
.search-bar {
  margin-top: 8px;
}
input {
  width: 100%;
  margin: 10px 0;
  max-width: 209px;
  padding: 13px 19px;
  background: #e5e5e5;
  border: 1px solid #d9d9d9;
  box-sizing: border-box;
  border-radius: 4px;
}
.additional-btns {
  margin-top: 19px;
  button {
    margin-left: 9px;
    padding: 7px 15px 8px 11px;
    font-size: 13px;
    line-height: 15px;
    color: #000000;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #fff0cd;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.25);
    border-radius: 3px;

    &:hover {
      background: rgba(255, 240, 205, 0.8);
    }

    svg {
      width: 20px;
      height: 20px;
      margin-right: 12px;
    }
  }
}
</style>
