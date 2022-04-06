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
        <div id="content-right">
          <div class="text-content">A.THÔNG TIN CHUNG:</div>
          <div class="m-dialog-body m-body-form">
            <div class="form-component">
              <div class="form-component-title">
                <b>Mã Nhân viên </b>(<span style="color: red">*</span>)
              </div>
              <div class="form-component-input">
                <input
                  @blur="validateDialog"
                  id="txtEmployeeCode"
                  required
                  validate="EmployeeCode"
                  v-model="employee.EmployeeCode"
                  property="EmployeeCode"
                  type="text"
                  class="input-field-form"
                />
              </div>
            </div>
            <div class="form-component">
              <div class="form-component-title">
                <b>Họ và tên </b>(<span style="color: red">*</span>)
              </div>
              <div class="form-component-input">
                <input
                  id="txtEmployeeName"
                  required
                  property="EmployeeName"
                  validate="EmployeeName"
                  type="text"
                  v-model="employee.EmployeeName"
                  class="input-field-form"
                  @blur="validateDialog"
                />
              </div>
            </div>
          </div>
          <div class="m-dialog-body m-body-form">
            <div class="form-component">
              <div class="form-component-title"><b>Ngày sinh</b></div>
              <div class="form-component-input">
                <input
                  :value="bindingDate(employee.DateOfBirth)"
                  @change="onChangeFormatDate"
                  type="date"
                  dateOfBirth
                  validate="EmployeeDateOfBirth"
                  property="DateOfBirth"
                  @blur="validateDialog"
                  class="input-field-form input-field-date"
                />
              </div>
            </div>
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
          </div>
          <div class="m-dialog-body m-body-form">
            <div class="form-component">
              <div class="form-component-title"><b>Số chứng minh thư</b></div>
              <div class="form-component-input">
                <input
                  @blur="validateDialog"
                  type="number"
                  class="input-field-form"
                  validate="EmployeeIdentifyNumber"
                />
              </div>
            </div>
            <div class="form-component">
              <div class="form-component-title"><b>Ngày cấp</b></div>
              <div class="form-component-input">
                <input type="date" class="input-field-form input-field-date" />
              </div>
            </div>
          </div>

          <div class="m-dialog-body m-body-form">
            <div class="form-component">
              <div class="form-component-title">
                <b>Email </b>(<span style="color: red">*</span>)
              </div>
              <div class="form-component-input">
                <input
                  id="txtEmail"
                  required
                  property="Email"
                  type="text"
                  class="input-field-form"
                  v-model="employee.Email"
                  validate="EmployeeEmail"
                  @blur="validateDialog"
                />
              </div>
            </div>
            <div class="form-component">
              <div class="form-component-title">
                <b>Số điện thoại </b>(<span style="color: red">*</span>)
              </div>
              <div class="form-component-input">
                <input
                  type="text"
                  class="input-field-form"
                  v-model="employee.PhoneNumber"
                  validate="EmployeeNumber"
                  @blur="validateDialog"
                />
              </div>
            </div>
          </div>
          <div class="text-content">B.THÔNG TIN CÔNG VIỆC:</div>
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
              <div class="form-component-title"><b>Mã số thuế các nhân</b></div>
              <div class="form-component-input">
                <input
                  v-model="employee.PersonalTaxCode"
                  type="number"
                  class="input-field-form"
                />
              </div>
            </div>
            <div class="form-component">
              <div class="form-component-title"><b>Mức lương cơ sở</b></div>
              <div class="form-component-input">
                <div class="currency-unit">(VNĐ)</div>
                <input
                  dir="rtl"
                  property="Salary"
                  type="number"
                  class="input-field-form"
                />
              </div>
            </div>
          </div>
          <div class="m-dialog-body m-body-form">
            <div class="form-component">
              <div class="form-component-title">
                <b>Ngày gia nhập công ty</b>
              </div>
              <div class="form-component-input">
                <input
                  :value="bindingDate(employee.CreatedDate)"
                  @change="onChangeFormatDate"
                  createdDate
                  type="date"
                  class="input-field-form input-field-date"
                />
              </div>
            </div>
            <div class="form-component">
              <div class="form-component-title"><b>Trạng thái làm việc</b></div>
              <div class="form-component-input">
                <select class="form-select" property="" name="" id="" value="">
                  <option value="0">Đang làm việc</option>
                  <option value="1">Đã nghỉ việc</option>
                </select>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="m-dialog-fotter m-fotter-form">
        <button id="btnCancel" @click="btnCloseDialog" class="m-btn m-cancel">
          Hủy
        </button>
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
  // hàm created
  created() {
    // lấy dữ liệu từ trên data
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
      showErrorDialog: this.CommonJs.showErrorDialog,
    };
  },

  methods: {
    // hàm reload data
    relaoadData() {
      this.$emit("reloadData");
    },
    // fomart date ở trong dialog detail
    onChangeFormatDate(e) {
      // kiểm tra nếu target chứa attibute "dateofbirth" thì gán employee.dateofbith = value
      if (e.target.hasAttribute("dateOfBirth")) {
        this.employee.DateOfBirth = e.target.value;
      // Nếu target chứa attibute "createdDate" thì gán employee.createdDate = value
      } else if (e.target.hasAttribute("createdDate")) {
        this.employee.CreatedDate = e.target.value;
      }
    },
    // fomart dữ liệu date do người dùng nhập vào
    bindingDate(date) {
      if (date) {
        date = new Date(date);
        // lây ra ngày
        let day = date.getDate();
        // lấy tháng
        let month = date.getMonth() + 1;
        // lấy năm
        let year = date.getFullYear();
        // thêm số 0 vào đằng trước
        day = day < 10 ? `0${day}` : day;
        month = month < 10 ? `0${month}` : month;
        date = year + "-" + month + "-" + day;
        return date;
      } else {
        return "";
      }
    },
    // close dialog
    btnCloseDialog() {
      this.$emit("closeOnClick", false);
    },

    // lưu dữ liệu
    btnSaveOnClick() {
      let employee = this.employee;
      // validate dữ liệu
      // 1.validate bắt buộc nhập
      // 2.validate khong để trống
      // 3.validate không dài quá
      // 4.validate định dạng email
      // thu thập thông tin nhân viên

      // gọi api lưu dữ liệu
      // Formmode = add
      if (this.formMode == this.MISAEnum.FormMode.Add) {
        axios
          .post("http://amis.manhnv.net/api/v1/Employees", employee)
          .then((response) => {
            console.log(response);
            // hiển thị toast thông báo thành công
            // ẩn form chi tiết và load lại dữ liẹu

            this.btnCloseDialog();
            // show toast
            this.showToastMsg("Thêm thành công");
            // load lại dữ liệu
            this.relaoadData();
          })
          .catch((error) => {
            this.showToastMsgErr(error.response.data.devMsg);
          });

      // formmode = put
      } else {
        axios
        // update dữ liệu employee
          .put(
            `http://amis.manhnv.net/api/v1/Employees/${employee.EmployeeId}`,
            employee
          )
          .then(() => {
            //show toast
            this.showToastMsg("Sửa thành công");
            // đóng dialog
            this.btnCloseDialog();
            // reload lại data
            this.relaoadData();
          })
          .catch((error) => {
            this.showToastMsgErr(error.response.data.devMsg);
          });
      }
    },
    // validate dialog
    validateDialog(event) {
      // lấy target và value
      var el = event.target;
      var value = el.value;
      // Chia trường hợp switch case của từng trường dữ liệu
      //1.Employeename
      //2.EmployeeCode
      //3.EmployeeEmail
      //4.EmployeeNumber
      //5.EmployeeIdentifyNumber
      //6.EmployeeDateOfBirth
      switch (el.getAttribute("validate")) {
        case "EmployeeName":
          if (!value) {
            //show dialog
            this.showErrorDialog("Vui lòng điền tên nhân viên");
            //add class error
            el.classList.add("error");
            // sau 3s remove class error
            setTimeout(() => {
              el.classList.remove("error");
            }, 3000);
          }
          break;
        case "EmployeeCode":
          if (!value) {
            this.showErrorDialog("Vui lòng điền mã nhân viên");
            //add class error

            el.classList.add("error");
            // sau 3s remove class error

            setTimeout(() => {
              el.classList.remove("error");
            }, 3000);
          }
          break;
        case "EmployeeEmail":
          var mailformat = /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/;
          if (!value) {
            this.showErrorDialog("Vui lòng điền Email");
            //add class error

            el.classList.add("error");
            // sau 3s remove class error

            setTimeout(() => {
              el.classList.remove("error");
            }, 3000);
          } else if (value) {
            //kiểm tra email có hợp lệ hay không
            if (!value.match(mailformat)) {
              this.showErrorDialog("Email không hợp lệ");
              el.classList.add("error");
              setTimeout(() => {
                el.classList.remove("error");
              }, 3000);
            }
          }
          break;
        case "EmployeeNumber":
          // kiểm tra điền sđt
          if (!value) {
            this.showErrorDialog("Vui lòng điền số điện thoại");
            el.classList.add("error");
            setTimeout(() => {
              el.classList.remove("error");
            }, 3000);
            // kiểm tra số điện thoại hợp lệ
          } else if (value) {
            if (!value.match(/^[+]*[(]{0,1}[0-9]{1,3}[)]{0,1}[-\s\./0-9]*$/g)) {
              this.showErrorDialog("Vui lòng chỉ điền số");
              el.classList.add("error");
              setTimeout(() => {
                el.classList.remove("error");
              }, 3000);
            }
          }
          break;
          // validate chứng minh thư
        case "EmployeeIdentifyNumber":
          if (!value) {
            this.showErrorDialog("Vui lòng điền số CTM/CCCD");
            el.classList.add("error");
            setTimeout(() => {
              el.classList.remove("error");
            }, 3000);
          }
          break;
          // validate ngày sinh nhật < ngày hiện tại
        case "EmployeeDateOfBirth":
          const currentYear = new Date().getFullYear();
          const currentMonth = new Date().getMonth() + 1;
          const currentDay = new Date().getDate();
          if (value) {
            let date = new Date(value);
            // lây ra năm sinh của người nhập
            let year = date.getFullYear();
            let month = date.getMonth() + 1;
            let day = date.getDate();
            // nếu năm lớn hơn hiện tại
            if (year > currentYear) {
              this.showErrorDialog("Ngày sinh không hợp lệ");
              el.classList.add("error");
              setTimeout(() => {
                el.classList.remove("error");
              }, 3000);
              el.value = "";
              // năm = nhau tháng lơn hơn hiện tại
            } else if (year == currentYear) {
              if (month > currentMonth) {
                this.showErrorDialog("Ngày sinh không hợp lệ");
                el.classList.add("error");
                setTimeout(() => {
                  el.classList.remove("error");
                }, 3000);
                el.value = "";
                // năm tháng = nhau và ngày lớn hơn hiện tại
              } else if (month == currentMonth) {
                if (day > currentDay) {
                  this.showErrorDialog("Ngày sinh không hợp lệ");
                  el.classList.add("error");
                  setTimeout(() => {
                    el.classList.remove("error");
                  }, 3000);
                  el.value = "";
                }
              }
            }
          }
        default:
          break;
      }
    },
  },
};
</script>
