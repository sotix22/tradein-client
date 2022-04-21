<template>
  <div class="check">
    <div class="check_block">
      <div class="check_block_header">
        <div class="check_block_header_title">
          <span>
            {{
              new Date(Date.parse(Request.DataCreateRequest)).toLocaleString(
                "ru",
                "yyyy MM dd"
              )
            }}</span
          >
        </div>
        <div class="check_block_header_about">
          <ul>
            <li>
              <p>Компания:</p>
              <p>Sotiks</p>
            </li>
            <li>
              <p>Точка:</p>
              <p>{{ Request.Retail }}</p>
            </li>
            <li>
              <p>Инициатор:</p>
              <p>{{ Request.Iniciator }}</p>
            </li>
            <li>
              <p>IMEI:</p>
              <p>{{ Request.IMEI }}</p>
            </li>
            <li>
              <p>Устройство:</p>
              <p>{{ Request.DeviceTitle }}</p>
            </li>
            <li>
              <p>Цена:</p>
              <p>{{ Request.Price }} Р</p>
            </li>
          </ul>
        </div>
      </div>
      <div class="check_block_content">
        <ul class="check_list">
          <liCheck
            v-for="(RateItem, index) in Rate"
            :key="index"
            :rateItem="RateItem"
          />
        </ul>
      </div>
      <div class="check_block_bottom">
        <div class="check_block_bottom_content">
          <p>Ф.И.О продавца:</p>
          <div class="output_manager">
            {{ Client?.firstName }} {{ Client?.lastName }}
            {{ Client?.secondName }}
          </div>
        </div>
        <div class="check_block_bottom_content">
          <p>Телефон продавца:</p>
          <div class="output_manager">{{ Client?.phone }}</div>
        </div>
      </div>
    </div>
    <el-button type="primary" @click="$emit('close')"
      >Закрыть чек лист</el-button
    >
  </div>
</template>
<script>
import liCheck from "./CheckItem/LiCheck.vue";
export default {
  components: {
    liCheck,
  },
  data() {
    return {};
  },
  methods: {
    print() {
      window.print();
    },
  },
  props: {
    Request: {
      type: Object,
    },
    Rate: {
      type: Object,
    },
    Client: {
      type: Object,
    },
  },
};
</script>
<style lang="scss" scoped>
.check {
  width: 100%;
  max-width: 500px;
  z-index: 100;
  background-color: #efefef;
  display: grid;
  margin: 0 auto;
  .check_block {
    display: grid;
    padding: 60px;
    .check_block_header {
      .check_block_header_title {
        text-align: center;
        margin-bottom: 20px;
      }
      .check_block_header_about {
        ul {
          padding: 0;
          margin-bottom: 40px;
          li {
            display: grid;
            grid-template-columns: auto 1fr;
            grid-gap: 5px;
          }
        }
      }
    }
    .check_block_content {
      padding: 20px;
      border-radius: 20px;
      background-color: white;
      .check_list {
        padding: 0;
      }
    }
  }
}
</style>
