<template>
  <q-card>
    <q-card-section>
      <div class="text-h6">Detail Rocket:{{ detailRocket.name }}</div>
    </q-card-section>

    <q-card-section class="q-pt-none">
      <div>Details:{{ detailRocket.details }}</div>
      <div>Rocket:{{ detailRocket.name }}</div>
      <div>
        Crew:{{ detailRocket.crew?.crew ? detailRocket.crew?.crew : "No crew" }}
      </div>
      <div>Launchpad:{{ detailRocket.launchpad }}</div>
    </q-card-section>

    <q-card-actions align="right">
      <q-btn flat label="OK" color="primary" v-close-popup />
    </q-card-actions>
  </q-card>
</template>

<script>
import { defineComponent } from "vue";
export default defineComponent({
  props: {
    detailRocket: Object,
  },
});
</script>
