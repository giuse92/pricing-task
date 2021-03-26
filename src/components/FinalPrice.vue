<template>
  <div style="color: green">
    {{
      customerPrice +
      orderPrice +
      (customerPrice + orderPrice) * additionalFieldFactor
    }}
    €/per month
  </div>
</template>

<script>
export default {
  name: "FinalPrice",
  props: ["customerPrice", "orderPrice", "additionalFieldFactor"],
};
</script>

<style></style>
