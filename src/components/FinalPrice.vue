<template>
  <div class="final-price">
    {{
      (customerPrice +
        orderPrice +
        (customerPrice + orderPrice) * additionalFieldFactor)
        | decimal
    }}
    €/al mese
  </div>
</template>

<script>
export default {
  name: "FinalPrice",
  props: ["customerPrice", "orderPrice", "additionalFieldFactor"],
  filters: {
    decimal(value) {
      return (value / 100).toFixed(2);
    },
  },
};
</script>

<style>
.final-price {
  font-size: 40px;
  font-weight: 600;
  line-height: 51px;
  text-align: center;
  margin-left: 20px;
}
</style>
