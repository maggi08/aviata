<template>
  <div class="filter">
    <div
      class="filter-box"
      v-for="(filter_type, filter_type_index) in filters"
      :key="filter_type_index"
    >
      <div class="filter-box__title">
        {{ filter_type.text }}
        <svg
          @mouseover="hover = `hover_${filter_type_index}`"
          @mouseleave="hover = `hover`"
          @click="resetTarif(filter_type.name)"
          width="20"
          height="20"
          viewBox="0 0 20 20"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M10.6464 8.64648L13.2929 6.00004L10.6464 3.35359L11.3535 2.64648L14 5.29293L16.6464 2.64648L17.3535 3.35359L14.7071 6.00004L17.3535 8.64648L16.6464 9.35359L14 6.70714L11.3535 9.35359L10.6464 8.64648ZM2.91465 6.00003H8C8 6.34074 8.0284 6.67482 8.08296 7.00003H2.91465C2.70873 7.58263 2.15311 8.00003 1.5 8.00003C0.671573 8.00003 0 7.32846 0 6.50003C0 5.6716 0.671573 5.00003 1.5 5.00003C2.15311 5.00003 2.70873 5.41743 2.91465 6.00003ZM7.91465 10H9.52779C9.86799 10.3801 10.2559 10.7166 10.6822 11H7.91465C7.70873 11.5826 7.15311 12 6.5 12C5.84689 12 5.29127 11.5826 5.08535 11H0.5C0.223858 11 0 10.7762 0 10.5C0 10.2239 0.223858 10 0.5 10H5.08535C5.29127 9.41743 5.84689 9.00003 6.5 9.00003C7.15311 9.00003 7.70873 9.41743 7.91465 10ZM2.91465 14C2.70873 13.4174 2.15311 13 1.5 13C0.671573 13 0 13.6716 0 14.5C0 15.3285 0.671573 16 1.5 16C2.15311 16 2.70873 15.5826 2.91465 15H14.5C14.7761 15 15 14.7762 15 14.5C15 14.2239 14.7761 14 14.5 14H2.91465Z"
            fill="#B9B9B9"
          />
        </svg>
        <div v-if="hover == `hover_${filter_type_index}`" class="tooltip">
          <div class="tooltip-box">Сбросить выбор</div>
        </div>
      </div>
      <div class="" v-for="(item, index) in filter_type.items" :key="index">
        <div v-if="filter[`${filter_type.name}`]" class="filter-box__input">
          <input
            v-model="filter[`${filter_type.name}`]"
            :value="item"
            :id="`${filter_type.name}_${index}`"
            type="checkbox"
          />
          <label :for="`${filter_type.name}_${index}`">{{ item.text }}</label>
        </div>
      </div>
    </div>

    <p @click="resetFilter" class="filter-reset">Сбросить все фильтры</p>
  </div>
</template>

<script>
export default {
  data: () => ({
    hover: "hover",
    filter: {
      airlines: [],
      tariffs: [],
    },
  }),
  props: {
    filters: Array,
  },
  methods: {
    resetTarif(name) {
      this.filter[name] = [];
    },
    resetFilter() {
      this.filter = {
        tariffs: [],
        airlines: [],
      };
    },
  },
};
</script>

<style lang="scss" scoped>
.filter {
  font-family: Open Sans;
  font-weight: normal;
  font-size: 12px;
  line-height: 16px;
  color: #202123;

  &-box {
    position: relative;
    padding: 12px 0;
    margin-bottom: 12px;
    background: #f5f5f5;
    border-radius: 4px;

    &__title {
      padding: 0 12px;
      display: flex;
      justify-content: space-between;
      font-weight: bold;
      font-size: 14px;
      line-height: 20px;
      margin-bottom: 2px;

      svg {
        transition: 0.22s;
        cursor: pointer;
        &:hover {
          path {
            fill: #7284e4;
          }
        }
      }
      .tooltip {
        position: absolute;
        top: 0;
        right: 24px;
        padding-bottom: 7px;
        transform: translate(50%, -100%);
        opacity: 0;
        animation: fade 0.6s forwards;
        &-box {
          width: 122px;
          height: 41px;
          background: #ffffff;
          border: 1px solid #e1e1e1;
          box-sizing: border-box;
          border-radius: 6px;
          display: flex;
          justify-content: center;
          align-items: center;

          font-weight: normal;
          font-size: 12px;
          line-height: 16px;

          &:after {
            content: " ";
            position: absolute;
            bottom: 0;
            left: 50%;
            width: 0;
            height: 0;
            transform: translate(-50%, 40%);
            border: 7px solid transparent;
            border-top-color: white;
          }
        }
      }
    }

    &__input {
      height: 32px;
      padding: 0 12px;
      display: flex;
      align-items: center;

      input {
        -webkit-appearance: none;
        width: 100%;
        max-width: 12px;
        height: 12px;
        background: #ffffff;
        border: 1px solid #b9b9b9;
        box-sizing: border-box;
        border-radius: 2px;
        transition: 0.22s;
        cursor: pointer;
        margin-right: 12px;
        &:active,
        &:checked {
          background: url("../assets/img/checkbox-active.svg") no-repeat center
            center;
        }
      }
      label {
        width: 100%;
        transition: 0.22s;
        cursor: pointer;
      }
      &:hover {
        background: #ebebeb;

        input {
          background: url("../assets/img/checkbox-hover.svg") no-repeat center
            center;
          &:checked {
            background: url("../assets/img/checkbox-active.svg") no-repeat
              center center;
          }
        }
      }
    }
  }

  &-reset {
    display: inline;
    color: #7284e4;
    padding-bottom: 2px;
    border-bottom: 1px dashed rgba(114, 132, 228, 0.5);
    cursor: pointer;
    transition: 0.22s;
    &:hover {
      border-bottom: 1px dashed rgba(114, 132, 228, 1);
    }
  }
}
@keyframes fade {
  100% {
    opacity: 1;
  }
}
</style>
