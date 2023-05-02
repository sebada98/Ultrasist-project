<template>
  <div>
    <SideBarKixComponent />
    <div class="container-fluid p-5 mx-3">
      <ChartFactsComponent/>
    </div>
  </div>
</template>

<script>
import SideBarKixComponent from "@/components/SideBarKixComponent.vue";
import ChartFactsComponent from "@/components/ChartFactsComponent.vue"

export default {
  components: {
    SideBarKixComponent,
    ChartFactsComponent
  },
};
</script>
