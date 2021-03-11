<template>
  <div class="h-full w-full container">
    <div class="w-full content-header" v-if="deleteMode">
      <div class="selected">
        Đã chọn <b>{{ itemsSelected.length }}</b>
      </div>
      <div
        class="unselected"
        @click="
          () => {
            deleteMode = false;
          }
        "
      >
        Bỏ chọn
      </div>
      <ms-button
        class="ms-button-danger"
        :iconStyle="{
          width: '20px',
          height: '20px',
          maskPosition: '-170px -26px',
          backgroundColor: 'var( --primary)',
        }"
        :buttonStyle="{
          marginRight: '5px',
          marginLeft: '-15px',
        }"
        text="Xóa"
        @onClick="onMutilDelete"
      ></ms-button>
    </div>
    <div class="w-full content-header" v-if="!deleteMode">
      <div class="content-header-title">Quản lý công tác</div>
      <div class="content-header-selectbox">
        Trạng thái:
        <div class="center-text">
          <DxSelectBox :items="status" v-model="statusValue" placeholder="" />
        </div>
      </div>
      <div class="content-header-right">
        <ms-input
          class="search"
          :prefix="require('../../../assets/icons/search.png')"
          placeholder="Tìm kiếm"
          :styles="{ width: '300px' }"
          :style="{ width: '300px' }"
        ></ms-input>
        <ms-button
          class="ms-button-primary"
          :iconStyle="{
            width: '16px',
            height: '16px',
            maskPosition: '-292px -76px',
           
          }"
          :buttonStyle="{
            marginRight: '5px',
            marginLeft: '-15px',
            marginTop: ' 4px',
          }"
          text="Thêm"
          @onClick="onCreate"
        ></ms-button>
        <ms-button
          class="ms-button-secondary"
          :iconStyle="{
            width: '16px',
            height: '18px',
            maskPosition: '-244px -51px',
            
          }"
          :buttonStyle="{
            marginTop: ' 4px',
          }"
          @onClick="onOpenFilter"
        ></ms-button>
      </div>
    </div>
    <div class=" w-full content-main">
      <ms-grid
        :data.sync="data"
        :class="[isFilter ? 'w-filter' : 'w-full']"
        :deleteMode="deleteMode"
        @onSelectionChanged="onSelectionRows"
        @onEdit="onEdit"
        @onDelete="onDelete"
      ></ms-grid>
      <ms-filter
        v-if="isFilter"
        :isFilter.sync="isFilter"
        :data="data.menu"
      ></ms-filter>
    </div>
    <div v-if="popupVisible">
      <DxPopup
        class="details"
        :width="900"
        :height="622"
        :visible="popupVisible"
        :v-if="popupVisible"
        :show-title="false"
        :close-on-outside-click="false"
        ><MissionAllowanceDetail
          :missionAllowanceId="isAdd ? 0 : missionAllowanceId"
          :onClosePopup="onClosePopup"
      /></DxPopup>
    </div>
  </div>
</template>

<script>
import data from "@/assets/json/mission-allowance.json";
import MissionAllowanceDetail from "@/views/management-request/mission-allowance/MissionAllowanceDetail";
export default {
  name: "mission-allowance-view",
  data() {
    return {
      isFilter: false,
      data: data,
      status: ["Tất cả", "Đã duyệt", "Chưa duyệt", "Từ chối"],
      statusValue: "Tất cả",
      popupVisible: false,
      deleteMode: false,
      itemsSelected: [],
      missionAllowanceId: 0,
      isAdd: true,
    };
  },
  components: { MissionAllowanceDetail },
  methods: {
    onEdit(e) {
      this.isAdd = false;
      this.missionAllowanceId = e;
      this.popupVisible = true;
    },
    onDelete(e) {
      alert(e);
    },
    onMutilDelete() {
      alert(this.itemsSelected);
    },
    onOpenFilter() {
      this.isFilter = !this.isFilter;
    },
    onCreate() {
      this.isAdd = true;
      this.popupVisible = true;
    },
    onClosePopup() {
      this.popupVisible = false;
    },
    onSelectionRows(e) {
      this.itemsSelected = JSON.parse(JSON.stringify(e));
      if (this.itemsSelected.length === 0) this.deleteMode = false;
      else this.deleteMode = true;
    },
  },
};
</script>

<style></style>
