<template>
  <div id="dlgDetail" class="m-dialog-box" :class="{ 'm-dialog-show': isShow }">
    <div class="m-dialog m-dialog-form">
      <div id="" @click="btnCloseDialog" class="m-dialog-close"></div>
      <div class="m-dialog-header">THÔNG TIN CHI TIẾT NHÂN VIÊN</div>
      <div class="form-body-dialog">
        <div class="left-content-dialog">
          <div class="img-dialog">
            <img src="../../assets/img/avatar-dialog.png" alt="" />
          </div>
          <div class="text-dialog">
            (Vui lòng chọn ảnh có định dang <br />.jpq,.jpeg,.png,.gif)
          </div>
        </div>
        <div>
          <div class="m-dialog-body m-body-form">
            <div class="form-component">
              <div class="form-component-title">
                <b>Mã Nhân viên</b>(<span class="red-text">*</span>)
              </div>
              <div class="form-component-input">
                <input
                  id="txtEmployeeCode"
                  required
                  v-model="employee.EmployeeCode"
                  property="EmployeeCode"
                  type="text"
                  class="input-field-form"
                />
              </div>
            </div>
            <div class="form-component">
              <div class="form-component-title">
                <b>Họ và tên</b>(<span class="red-text">*</span>)
              </div>
              <div class="form-component-input">
                <input
                  id="txtEmployeeName"
                  required
                  property="EmployeeName"
                  type="text"
                  v-model="employee.EmployeeName"
                  class="input-field-form"
                  @blur="validateEmty"
                />
              </div>
            </div>
          </div>
          <div class="m-dialog-body m-body-form">
            <div class="form-component">
              <div class="form-component-title"><b>Giới tính</b></div>
              <div class="form-component-input">
                <select
                  class="form-select"
                  property="Gender"
                  name=""
                  id=""
                  v-model="employee.Gender"
                >
                  <option value="0">Nam</option>
                  <option value="1">Nữ</option>
                  <option value="2">Khác</option>
                </select>
              </div>
            </div>
            <div class="form-component">
              <div class="form-component-title"><b>Ngày sinh</b></div>
              <div class="form-component-input">
                <input
                  type="date"
                  property="DateOfBirth"
                  class="input-field-form input-field-date"
                  v-model="employee.DateOfBirth"
                />
              </div>
            </div>
          </div>

          <div class="m-dialog-body m-body-form">
            <div class="form-component">
              <div class="form-component-title"><b>Email</b></div>
              <div class="form-component-input">
                <input
                  id="txtEmail"
                  required
                  property="Email"
                  type="text"
                  class="input-field-form"
                  v-model="employee.Email"
                  @blur="validateEmty"
                />
              </div>
            </div>
            <div class="form-component">
              <div class="form-component-title"><b>Số điện thoại</b></div>
              <div class="form-component-input">
                <input property="Mobile" type="text" class="input-field-form" />
              </div>
            </div>
          </div>

          <div class="m-dialog-body m-body-form">
            <div class="form-component">
              <div class="form-component-title"><b>Vị trí</b></div>
              <div class="form-component-input">
                <select
                  class="form-select"
                  property="PositionId"
                  name=""
                  id=""
                  v-model="employee.PositionId"
                >
                  <option value="0">Giám đốc</option>
                  <option value="1">Trưởng phòng</option>
                  <option value="2">Nhân viên</option>
                </select>
              </div>
            </div>
            <div class="form-component">
              <div class="form-component-title"><b>Phòng ban</b></div>
              <div class="form-component-input">
                <select
                  class="form-select"
                  property="DepartmentId"
                  name=""
                  id=""
                  v-model="employee.DepartmentId"
                >
                  <option
                    v-for="dep in departments"
                    :key="dep.DepartmentId"
                    :value="dep.DepartmentId"
                  >
                    {{ dep.DepartmentName }}
                  </option>
                  <!-- <option value="6f0731d0-a27e-11ec-9b48-00163e06abee">
                  Phòng Nhân sự
                </option>
                <option value="17120d02-6ab5-3e43-18cb-66948daf6128">
                  Phòng kế toán
                </option>
                <option value="142cb08f-7c31-21fa-8e90-67245e8b283e">
                  Phòng Marketing
                </option> -->
                </select>
              </div>
            </div>
          </div>

          <div class="m-dialog-body m-body-form">
            <div class="form-component">
              <div class="form-component-title"><b>Mức lương cơ sở</b></div>
              <div class="form-component-input">
                <input property="Salary" type="text" class="input-field-form" />
              </div>
            </div>
          </div>
          <div class="m-dialog-body m-body-form">
            <div class="form-component">
              <div class="form-component-title"><b>Địa chỉ</b></div>
              <div class="form-component-input form-component-line">
                <textarea
                  property="Address"
                  type="text"
                  class="input-field-form input-field-line"
                ></textarea>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="m-dialog-fotter m-fotter-form">
        <button id="btnCancel" class="m-btn m-cancel">Hủy</button>
        <button id="btn-save" class="m-btn m-btn-icon" @click="btnSaveOnClick">
          Lưu
        </button>
      </div>
    </div>
  </div>
</template>

<script>
/*eslint-disable */
import MISAEnum from "../../js/enum.js";
import MISAResource from "../../js/resource.js";
import axios from "axios";
console.log(MISAResource, MISAEnum);
export default {
  props: [
    "isShow",
    "employeeSelectedInChil",
    "formMode",
    "showToastMsgErr",
    "showToastMsg",
    "showDialogMsg",
    "showConfirm",
  ],
  watch: {
    employeeSelectedInChil: function (newValue) {
      this.employee = newValue;
    },
  },
  created() {
    let me = this;
    axios
      .get("http://amis.manhnv.net/api/v1/Departments")
      .then(function (response) {
        me.departments = response.data;
      })
      .catch(function (error) {
        showToastMsgErr(error);
      });
  },
  data() {
    return {
      employee: {},
      departments: [],
      positions: [],
    };
  },

  methods: {
    relaoadData() {
      this.$emit("reloadData");
    },
    btnCloseDialog() {
      this.$emit("closeOnClick", false);
    },
    btnSaveOnClick() {
      let employee = this.employee;
      // validate dữ liệu
      // 1.validate bắt buộc nhập
      // 2.validate khong để trống
      // 3.validate không dài quá
      // 4.validate định dạng email
      // thu thập thông tin nhân viên

      // gọi api lưu dữ liệu
      if (this.formMode == this.MISAEnum.FormMode.Add) {
        axios
          .post("http://amis.manhnv.net/api/v1/Employees", employee)
          .then((response) => {
            console.log(response);
            // hiển thị toast thông báo thành công
            // ẩn form chi tiết và load lại dữ liẹu

            this.btnCloseDialog();
            this.showToastMsg("Thêm thành công");
            console.log(123)
            this.relaoadData();
          })
          .catch((error) => {
            this.showToastMsgErr(error.response.data.devMsg);
          });
      } else {
        axios
          .put(
            `http://amis.manhnv.net/api/v1/Employees/${employee.EmployeeId}`,
            employee
          )
          .then(() => {
            this.showToastMsg("Sửa thành công");
            this.btnCloseDialog();
            this.relaoadData()
          })
          .catch((error) => {

            this.showToastMsgErr(error.response.data.devMsg);
          });
      }
    },
    validateEmty() {
      var el = event.currentTarget;
      var value = el.value;
      if (!value) {
        el.classList.add("error");
      }
    },
  },
};
</script>
