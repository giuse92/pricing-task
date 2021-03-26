<template>
  <section v-if="isLoaded">
    <div class="block">
      <span class="demonstration">Customers</span>
      <el-slider
        v-model="customersValue"
        :min="0"
        :max="4"
        :step="1"
        :format-tooltip="changeCustomersTooltip"
        show-stops
      >
      </el-slider>
    </div>
    <div style="margin-top: 50px" class="block">
      <span class="demonstration">Orders</span>
      <el-slider
        v-model="ordersValue"
        :format-tooltip="changeOrdersTooltip"
        :min="0"
        :max="4"
        :step="1"
        show-stops
      >
      </el-slider>
    </div>
    <div class="block">
      <el-input-number
        :min="5"
        :max="Infinity"
        v-model="additionalFieldsValue"
        :controls="true"
      >
      </el-input-number>
    </div>
    <el-button-group>
      <el-button @click="changeFrequency" name="month" type="success" plain
        >Month</el-button
      >
      <el-button @click="changeFrequency" name="quarter" type="success" plain
        >Quarter</el-button
      >
      <el-button
        @click="changeFrequency"
        name="year"
        type="success"
        autofocus
        plain
        >Year</el-button
      >
    </el-button-group>
    <FinalPrice
      :customerPrice="customersStepsPrice"
      :orderPrice="ordersStepsPrice"
      :additionalFieldFactor="
        additionalFieldFactor *
        (additionalFieldsValue - defaultAdditionalFields)
      "
    />
  </section>
</template>

<script>
import axios from "axios";
import FinalPrice from "../components/FinalPrice";
export default {
  data() {
    return {
      isLoaded: false,
      customersValue: 0,
      ordersValue: 0,
      customersSteps: [],
      ordersSteps: [],
      additionalFieldsValue: 0,
      defaultAdditionalFields: 0,
      additionalFieldFactor: 0,
      frequency: "year",
    };
  },
  components: {
    FinalPrice,
  },
  created() {
    const API_URL = "https://backend.rfmcube.com/api/fe/pricing";
    axios
      .get(API_URL)
      .then((json) => {
        this.isLoaded = true;
        this.customersSteps = json.data.pricingConfig.customersSteps;
        this.ordersSteps = json.data.pricingConfig.ordersSteps;
        this.defaultAdditionalFields =
          json.data.pricingConfig.defaultAdditionalFields;
        this.additionalFieldFactor =
          json.data.pricingConfig.additionalFieldFactor;
      })
      .catch((err) => console.log(err));
  },
  methods: {
    changeCustomersTooltip(val) {
      switch (val) {
        case 0:
          val = "500";
          break;
        case 1:
          val = "5k";
          break;
        case 2:
          val = "30k";
          break;
        case 3:
          val = "100k";
          break;
        case 4:
          val = "200k";
          break;
      }
      return val;
    },
    changeOrdersTooltip(val) {
      switch (val) {
        case 0:
          val = "50";
          break;
        case 1:
          val = "300";
          break;
        case 2:
          val = "1k";
          break;
        case 3:
          val = "3k";
          break;
        case 4:
          val = "6k";
          break;
      }
      return val;
    },
    changeFrequency(e) {
      this.frequency = e.currentTarget.name;
    },
  },
  computed: {
    customersStepsPrice: function (price) {
      switch (this.frequency) {
        case "year":
          price = this.customersSteps[this.customersValue].prices.year;
          break;
        case "quarter":
          price = this.customersSteps[this.customersValue].prices.quarter;
          break;
        case "month":
          price = this.customersSteps[this.customersValue].prices.month;
          break;
        default:
          price = this.customersSteps[this.customersValue].prices.year;
          break;
      }
      return price;
    },
    ordersStepsPrice: function (price) {
      switch (this.frequency) {
        case "year":
          price = this.ordersSteps[this.ordersValue].prices.year;
          break;
        case "quarter":
          price = this.ordersSteps[this.ordersValue].prices.quarter;
          break;
        case "month":
          price = this.ordersSteps[this.ordersValue].prices.month;
          break;
        default:
          price = this.ordersSteps[this.ordersValue].prices.year;
          break;
      }
      return price;
    },
  },
};
</script>
