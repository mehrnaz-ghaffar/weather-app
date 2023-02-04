<template>
  <div class="card">
    <h1 
        class="cardTitle"
    >
        hi {{ msg }}
    </h1>

    <div   
        class="cardDetails"
    >
      Lorem ipsum dolor, sit amet consectetur adipisicing elit. Numquam natus
      incidunt ducimus hic nam error odit, assumenda neque nulla. Quaerat labore
      consequuntur ab facere. Saepe dolor laudantium neque sequi tempora?
    </div>
  </div>
</template>

<script>
export default {
  name: "iconDetailsCard",
  props: ["msg"],
};
</script>

<style scoped>
.card {
  transition: all 0.8s ease;
}

.cardTitle{
    max-height: 60px;
    background-color: rgb(88, 88, 214);
}
</style>
