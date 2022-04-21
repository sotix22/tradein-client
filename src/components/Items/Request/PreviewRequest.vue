<template>
  <el-dialog
    v-model="value"
    :title="request.DeviceTitle || 'Не известно'"
    center
  >
    <template #default>
      <el-dialog
        append-to-body
        v-model="viewCheck"
        title="Чек-лист"
        width="60%"
        center
      >
        <template #default>
          <Check
            @close="viewCheck = false"
            :Request="request"
            :Client="request.Client"
            :Rate="request.Rate"
          />
        </template>
      </el-dialog>
      <div class="preview">
        <!-- <div class="status"><el-result icon="error" /></div> -->

        <div class="preview__img">
          <el-image :src="img">
            <template #placeholder>
              <div class="image-slot">Loading<span class="dot">...</span></div>
            </template>
          </el-image>
        </div>
        <div class="preview__content">
          <ul>
            <li>
              <p>
                Адрес магазина:
                <span>{{ request?.Retail || "Не известно" }}</span>
              </p>
            </li>
            <li>
              <p>
                ФИО продавца:
                <span
                  >{{ request?.Client?.firstName || "Не известно" }}
                  {{ request?.Client?.lastName || "Не известно" }}
                  {{ request?.Client?.secondName || "Не известно" }}</span
                >
              </p>
            </li>
            <li>
              <p>
                IMEI устройства:
                <span>{{ request?.IMEI || "Не известно" }}</span>
              </p>
            </li>

            <li class="right">
              <hr />
              <strong>
                Оценочная стоимость:
                <span>{{ request?.Price || "Не рассчитано" }}руб.</span>
              </strong>
            </li>
          </ul>
        </div>
      </div>
      <el-dialog
        v-model="innerVisible"
        width="10%"
        title="Inner Dialog"
        append-to-body
      />
    </template>
    <template #footer>
      <div class="dialog-footer">
        <el-button type="warning" circle @click="syncCheck(id)">
          <el-icon :size="20"><refresh /></el-icon>
        </el-button>
        <el-button type="primary" @click="viewCheck = true"
          >Открыть чек лист</el-button
        >
      </div>
    </template>
  </el-dialog>
</template>
<script>
import { ElNotification, ElResult } from "element-plus";
import { Refresh } from "@element-plus/icons-vue";
import Check from "../../Device/CheckPreview.vue";
import { config } from ".././../../api/constant.js";
import axios from "axios";
import img from "../../../../public/img/Device.jpg";
export default {
  name: "Preview",
  data() {
    return {
      img: img,
      imgStatus: false,
      request: "",
      CheckShow: false,
      Sync: false,
      viewCheck: false,
    };
  },
  mounted() {
    axios({
      method: "POST",
      mode: "no-cors",
      headers: {
        "Access-Control-Allow-Origin": "*",
      },
      url: `${config.url}/request/getrequestsbyid`,
      data: {
        id: this.$props.id,
      },
    }).then((res) => {
      this.request = res.data;
      // this.img = `https://sotix-tradein-server.herokuapp.com/public/img/${res.data.DeviceId}.jpg`;
    });
  },
  computed: {
    view: {
      get() {
        return this.value;
      },
      set(value) {
        this.$emit("input", value);
      },
    },
  },
  methods: {
    CloseCheck() {
      this.CheckShow = false;
    },
    openCheck() {
      this.CheckShow = true;
    },
    onLoadHandler(value) {
      this.imgStatus = true;
    },
    close() {
      this.$emit("CloseE");
    },
    async syncCheck(id) {
      try {
        console.log(id);
        await axios({
          method: "POST",
          mode: "no-cors",
          headers: {
            "Access-Control-Allow-Origin": "*",
          },
          url: `${config.url}/request/sync`,
          data: {
            id: id,
          },
        });
        ElNotification.success({
          title: "Успешно",
          message: "Заявка синхронизирована с 1с",
          offset: 100,
          duration: 2000,
        });
      } catch (e) {
        ElNotification({
          title: "Ошибка",
          message: e,
          offset: 100,
          duration: 4000,
          type: "error",
        });
        console.error(e);
      }
    },
  },
  components: { Check, Refresh, ElResult },
  props: {
    id: String,
    Img: String,
    Title: String,
    Address: String,
    Client: String,
    Imei: String,
    Price: String,
    value: {
      type: Boolean,
      default: false,
    },
  },
};
</script>
<style lang="scss" scoped>
.status {
  position: absolute;
  left: 0%;
  top: 100%;
}
hr {
  margin-top: 10px;
  margin-bottom: 10px;
}
.preview {
  position: relative;
  display: grid;
  justify-content: center;
  &__image {
    display: grid;
  }
}
.right {
  text-align: right;
}
.is-circle {
  border-radius: 50%;
  padding: 8px;
}
</style>
