<template>
  <q-page class="flex flex-center">
    <q-card>
      <q-tabs
        v-model="tab"
        dense
        class="text-grey"
        active-color="primary"
        indicator-color="primary"
        align="justify"
        narrow-indicator
      >
        <q-tab name="all" label="All" @click="filterRocketData(1)" />
        <q-tab name="last" label="Last" @click="filterRocketData(2)" />
        <q-tab name="upcoming" label="Upcoming" @click="filterRocketData(3)" />
      </q-tabs>

      <q-separator />

      <q-tab-panels v-model="tab" animated>
        <q-tab-panel name="all">
          <div
            class="text-h6"
            v-for="(item, index) in onFilterData"
            :key="index"
            @click="onOpenDialog(item)"
          >
            <div>Name:{{ item?.name }}</div>
            <div>
              Time:{{
                item?.static_fire_date_utc
                  ? item?.static_fire_date_utc
                  : "No time"
              }}
            </div>

            <div>
              Picture:<q-img
                class="w-full max-h-[330px] pl-0"
                :src="item?.links?.patch?.small"
              />
            </div>
            <div>Crews:{{ item?.crew ? item?.crew.length : "0" }}</div>
            <q-separator />
          </div>
        </q-tab-panel>

        <q-tab-panel name="last">
          <div
            class="text-h6"
            v-for="(item, index) in onFilterData"
            :key="index"
            @click="onOpenDialog(item)"
          >
            <div>Name:{{ item?.name }}</div>
            <div>
              Time:{{
                item?.static_fire_date_utc
                  ? item?.static_fire_date_utc
                  : "No time"
              }}
            </div>

            <div>
              Picture:<q-img
                class="w-full max-h-[330px] pl-0"
                :src="item?.links?.patch?.small"
              />
            </div>
            <div>Crews:{{ item?.crew ? item?.crew.length : "0" }}</div>
            <q-separator />
          </div>
        </q-tab-panel>

        <q-tab-panel name="upcoming">
          <div
            class="text-h6"
            v-for="(item, index) in onFilterData"
            :key="index"
            @click="onOpenDialog(item)"
          >
            <div>Name:{{ item?.name }}</div>
            <div>
              Time:{{
                item?.static_fire_date_utc
                  ? item?.static_fire_date_utc
                  : "No time"
              }}
            </div>

            <div>
              Picture:<q-img
                class="w-full max-h-[330px] pl-0"
                :src="item?.links?.patch?.small"
              />
            </div>
            <div>Crews:{{ item?.crew ? item?.crew.length : "0" }}</div>
            <q-separator />
          </div>
        </q-tab-panel>
      </q-tab-panels>

      <q-dialog v-model="detail">
        <ModalDetail
          class="w-[700px] max-w-[90%]"
          :detailRocket="onSelectedItem"
        />
      </q-dialog>
    </q-card>
  </q-page>
</template>

<script>
import axios from "axios";
import { defineComponent, onMounted, ref } from "vue";
import ModalDetail from "src/pages/modalDetail.vue";

export default defineComponent({
  name: "IndexPage",

  components: {
    ModalDetail,
  },
  setup() {
    const data = ref();
    const onFilterData = ref();
    const detail = ref(false);
    const onSelectedItem = ref();
    const getData = async () => {
      data.value = await axios.get("https://api.spacexdata.com/v5/launches");
      onFilterData.value = data.value.data;
    };
    const filterRocketData = (type) => {
      if (type == 1) {
        onFilterData.value = data.value.data;
      } else if (type == 2) {
        onFilterData.value = data.value.data.filter((x) => x.upcoming == false);
      } else {
        onFilterData.value = data.value.data.filter((x) => x.upcoming == true);
      }
    };

    const onOpenDialog = (item) => {
      onSelectedItem.value = item;
      detail.value = true;
    };

    onMounted(() => {
      getData();
    });
    return {
      tab: ref("all"),
      filterRocketData,
      onFilterData,
      onSelectedItem,
      onOpenDialog,
      detail,
    };
  },
});
</script>
