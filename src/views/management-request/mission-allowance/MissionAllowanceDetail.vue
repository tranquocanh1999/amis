<template>
  <div class="mission-allowance-detail">
    <div class="detail-title">
      <div class="detail-text">
        <div style="font-size:24px;font-weight:bolder;margin-top:-5px ;">
          {{ title }}
        </div>
        - Công tác
      </div>
      <div class="detail-icon-cancel" @click="onClosePopup"></div>
    </div>
    <div class="detail-content">
      <div class="detail-form">
        <div class="left">
          <ms-field
            label="Người đề nghị"
            targetID="proposer"
            :messageErr="messageErr.proposer"
            required
          >
            <DxSelectBox
              :value.sync="defaultApplication.proposer"
              placeholder=""
              :search-enabled="true"
              :data-source="employees.data"
              searchModeOption="contains"
              searchExprOption="Name"
              searchTimeoutOption="200"
              minSearchLengthOption="0"
              :show-data-before-search="true"
              display-expr="name"
              value-expr="id"
              ref="autoFocus"
            >
            </DxSelectBox>
          </ms-field>
          <ms-field label="Đơn vị công tác">
            <DxTextBox :disabled="true" value=" " />
          </ms-field>
          <ms-field
            label="Người duyệt"
            targetID="acceptedBy"
            :messageErr="messageErr.acceptedBy"
            required
          >
            <DxSelectBox
              :value.sync="defaultApplication.acceptedBy"
              placeholder=""
              :search-enabled="true"
              :data-source="employees.data"
              searchModeOption="contains"
              searchExprOption="Name"
              searchTimeoutOption="200"
              minSearchLengthOption="0"
              :show-data-before-search="true"
              display-expr="name"
              value-expr="id"
            />
          </ms-field>
          <ms-field
            label="Ngày đề nghị"
            targetID="recommendedDate"
            :messageErr="messageErr.recommendedDate"
            required
          >
            <ms-date-picker
              :datetime.sync="defaultApplication.recommendedDate"
            ></ms-date-picker>
          </ms-field>
          <ms-field
            label="Ngày đi"
            targetID="departureDay"
            :messageErr="messageErr.departureDay"
            required
          >
            <ms-date-picker
              :datetime.sync="defaultApplication.departureDay"
            ></ms-date-picker>
          </ms-field>
          <ms-field
            label="Ngày về"
            targetID="dateBack"
            :messageErr="messageErr.dateBack"
            required
          >
            <ms-date-picker
              :datetime.sync="defaultApplication.dateBack"
            ></ms-date-picker>
          </ms-field>
          <ms-field label="Địa điểm công tác">
            <DxTextBox value="" />
          </ms-field>
          <ms-field label="Lí do công tác">
            <DxTextArea :height="90" />
          </ms-field>
        </div>
        <div class="right">
          <ms-field label="Người liên quan">
            <DxTagBox
              :items="employees"
              :search-enabled="true"
              :height="90"
              placeholder=""
              display-expr="name"
              value-expr="id"
            />
          </ms-field>
          <ms-field label="Yêu cầu hỗ trợ">
            <DxTextArea :height="90" />
          </ms-field>
          <ms-field label="Người hỗ trợ">
            <DxTagBox
              :items="employees"
              :search-enabled="true"
              :height="90"
              placeholder=""
              display-expr="name"
              value-expr="id"
              :scroll-by-thumb="true"
            />
          </ms-field>
          <ms-field label="Ghi chú">
            <DxTextArea :height="90" />
          </ms-field>
          <ms-field label="Trạng thái" required>
            <DxSelectBox
              :value.sync="defaultApplication.statusID"
              placeholder=""
              :search-enabled="true"
              :data-source="missionAllowance.status"
              searchModeOption="contains"
              searchExprOption="Name"
              searchTimeoutOption="200"
              minSearchLengthOption="0"
              :show-data-before-search="true"
              display-expr="name"
              value-expr="id"
            />
          </ms-field>
        </div>
      </div>
      <div class="w-full" style=" margin-top:10px">
        <b style="font-size:18px;">Nhân viên đi cùng</b>
      </div>
      <div class="content-nodata">
        <div class="nodata"><i>Chưa có dữ liệu</i></div>
        <ms-button
          class="ms-button-filled"
          :iconStyle="{
            width: '16px',
            height: '16px',
            maskPosition: '-292px -76px',
            backgroundColor: '#3C4ADA',
            marginRight: '10px',
          }"
          :buttonStyle="{
            marginRight: '5px',
            marginLeft: '-15px',
          }"
          text="Chọn"
        ></ms-button>
      </div>
    </div>
    <div class="detail-footer">
      <div class="event">
        <ms-button
          class="ms-button-secondary"
          :buttonStyle="{
            marginRight: '20px',
          }"
          text="Hủy"
          @onClick="onClosePopup"
        ></ms-button>
        <ms-button
          class="ms-button-primary"
          :buttonStyle="{
            marginRight: '20px',
          }"
          text="Lưu"
          @onClick="onSubmit"
        ></ms-button>
      </div>
    </div>
  </div>
</template>

<script>
import missionAllowance from "@/assets/json/mission-allowance.json";
import employees from "../../../assets/json/employee.json";

export default {
  name: "mission-allowance-detail",
  props: {
    onClosePopup: {
      type: Function,
      default: () => {},
    },
    missionAllowanceId: { type: Number, default: 0 },
  },
  data() {
    return {
      title: "Thêm đơn",
      missionAllowance: missionAllowance,
      employees: employees,
      defaultApplication: {
        id: 15,
        proposer: "",
        recommendedDate: this.getDateNow(),
        departureDay: "",
        dateBack: "",
        workplace: "",
        reasonForWork: "",
        acceptedBy: "",
        statusID: 1,
        peopleInvolved:""
      },

      messageErr: {
        proposer: "",
        acceptedBy: "",
        recommendedDate: "",
        departureDay: "",
        dateBack: "",
      },
    };
  },
  methods: {
    setMissionAllowanceId(id) {
      var result = this.missionAllowance.data.find(
        (element) => element.id == id
      );

      this.defaultApplication = JSON.parse(JSON.stringify(result));
    },
    onSave() {
      console.log(this.defaultApplication);
    },
    onSubmit() {
      this.validate();
    },
    validate() {
     
      this.messageErr.proposer = "";
      this.messageErr.acceptedBy = "";
      this.messageErr.recommendedDate = "";
      this.messageErr.departureDay = "";
      this.messageErr.dateBack = "";
      if (this.defaultApplication.proposer === "")
        this.messageErr.proposer = "Người đề nghị không được để trống.";
      if (this.defaultApplication.acceptedBy === "")
        this.messageErr.acceptedBy = "Người duyệt không được để trống.";
      if (this.defaultApplication.recommendedDate === "")
        this.messageErr.recommendedDate = "Ngày đề nghị không được để trống.";
      if (this.defaultApplication.departureDay === "")
        this.messageErr.departureDay = "Ngày đi không được để trống.";
      if (this.defaultApplication.dateBack === "")
        this.messageErr.dateBack = "Ngày về không được để trống.";
      if (
        this.dateCompare(
          this.defaultApplication.recommendedDate,
          this.defaultApplication.departureDay
        ) > 0
      )
        this.messageErr.departureDay = "Ngày đi không được nhỏ hơn ngày đề nghị.";
        if (
        this.dateCompare(
          
          this.defaultApplication.departureDay,
          this.defaultApplication.dateBack
        ) > 0
      )
        this.messageErr.dateBack = "Ngày về không được nhỏ hơn ngày đi.";
      
    },
    getDateNow() {
      var now = new Date();
      var day = ("0" + now.getDate()).slice(-2);
      var month = ("0" + (now.getMonth() + 1)).slice(-2);
      var hour = ("0" + now.getHours()).slice(-2);
      var minute = ("0" + now.getMinutes()).slice(-2);
      var today =
        day +
        "/" +
        month +
        "/" +
        +now.getFullYear() +
        " " +
        hour +
        ":" +
        minute;
      return today;
    },

    dateCompare(a, b) {
      var date1 = new Date(this.formatDate(a));
      var date2 = new Date(this.formatDate(b));

      return date1 - date2;
    },
    formatDate(a) {
      return a.slice(3, 5) + "/" + a.slice(0, 2) + "/" + a.slice(6);
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.$refs.autoFocus.instance.focus();
      setTimeout(() => {
        this.$refs.autoFocus.instance.focus();
      }, 1000);
    });

    if (this.missionAllowanceId !== 0) this.title = "Sửa đơn";
    this.setMissionAllowanceId(this.missionAllowanceId);
  },
};
</script>

<style>
.dx-state-focused {
  border: 0 !important;
  outline: 0 !important;
}
</style>
