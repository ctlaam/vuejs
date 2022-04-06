<template>
  <div style="height: 100%">
    <div class="m-page-header">
      <div class="m-page-title">DANH SÁCH NHÂN VIÊN</div>
      <div class="m-page-btn">
        <button @click="btnAddOnClick" id="btn-add" class="m-btn m-btn-icon">
          <img class="m-icon-btn" src="../../assets/icon/add.png" alt="" />
          <div class="text-btn">Thêm nhân viên</div>
        </button>
      </div>
    </div>
    <div class="m-page-tool-bar">
      <div class="m-toolbar-left">
        <input
          type="text"
          class="m-input"
          placeholder="Tìm kiếm"
          v-model="Name"
        />
        <select class="m-combobox">
          <option value="0">Nam</option>
          <option value="1">Nữ</option>
          <option value="2">Khác</option>
        </select>
        <select class="m-combobox">
          <option value="0">Nam</option>
          <option value="1">Nữ</option>
          <option value="2">Khác</option>
        </select>
      </div>
      <div class="m-toolbar-right">
        <button
          id="btnDelete"
          class="m-btn-delete"
          @click="ComfirmDeleteEmployee"
        ></button>
        <button @click="reloadData" class="m-btn-refresh"></button>
      </div>
    </div>
    <div class="m-grid">
      <div class="m-grid-table">
        <table id="tblEmployeeList" class="m-table">
          <thead>
            <tr>
              <th class="text-align-left" style="width: 50px" propName="STT">
                STT
              </th>
              <th
                class="text-align-left"
                style="width: 200px"
                propName="DepartmentName"
              >
                Phòng ban
              </th>
              <th
                class="text-align-left"
                style="width: 150px"
                propName="EmployeeCode"
              >
                Mã nhân viên
              </th>
              <th
                class="text-align-left"
                propName="EmployeeName"
                style="width: 220px"
              >
                Họ và tên
              </th>
              <th
                class="text-align-left"
                style="width: 150px"
                propName="GenderName"
              >
                Giới tính
              </th>
              <th
                class="text-align-left"
                style="width: 150px"
                propName="EmployeePosition"
              >
                Vị trí
              </th>
              <th
                class="text-align-left"
                style="width: 150px"
                propName="TelephoneNumber"
              >
                Số điện thoại
              </th>
              <th class="text-align-left" style="width: 150px" propName="Email">
                Email
              </th>
              <th
                class="text-align-center"
                style="width: 150px"
                propName="DateOfBirth"
                format="Date"
              >
                Ngày sinh
              </th>
              <th
                class="text-align-right"
                style="width: 150px"
                propName="Salary"
                format="Money"
              >
                Mức lương
              </th>
            </tr>
          </thead>
          <tbody class="tbody">
            <tr
              v-for="(emp, index) in employees"
              :key="emp.EmployeeId"
              @dblclick="rowOnDblClick(emp)"
              @click="rowOnClick($event, emp)"
            >
              <td>{{ index + 1 }}</td>
              <td>{{ emp.DepartmentName }}</td>
              <td>{{ emp.EmployeeCode }}</td>
              <td>{{ emp.EmployeeName }}</td>
              <td>{{ renderGender(emp.Gender) }}</td>
              <td>{{ emp.EmployeePosition || "Chưa có" }}</td>
              <td>{{ emp.TelephoneNumber || "Chưa có" }}</td>
              <td>{{ emp.Email || "Chưa có" }}</td>
              <td class="text-align-center">
                {{ formatDate(emp.DateOfBirth) || "Chưa có" }}
              </td>
              <td class="text-align-right">
                {{ formatMoney(emp.Salary) || "Chưa có" }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="m-paging">
        <div class="display-page">
          <div class="text-display-page">
            Hiển thị <b>01-20/123</b> lao động
          </div>
        </div>
        <div class="paging">
          <div class="start-page">
            <i class="fa-solid fa-angles-left"></i>
          </div>
          <div class="prev-page">
            <i class="fa-solid fa-angle-left"></i>
          </div>
          <ul class="number-pages">
            <li class="page page-selected"><button>1</button></li>
            <li class="page"><button>2</button></li>
            <li class="page"><button>3</button></li>
            <li class="page"><button>4</button></li>
          </ul>
          <div class="next-page">
            <i class="fa-solid fa-chevron-right"></i>
          </div>
          <div class="end-page">
            <i class="fa-solid fa-angles-right"></i>
          </div>
        </div>
        <div class="explain">
          <div class="people-page">
            <p><b>20</b> lao động/trang</p>
          </div>
        </div>
      </div>
    </div>
    <EmployeeDetail
      :isShow="isShowDialog"
      @formatDate="formatDate"
      @closeOnClick="showOrHideDialog"
      @reloadData="reloadData"
      :employeeSelectedInChil="employeeSelected"
      :formMode="formMode"
      :showToastMsgErr="showToastMsgErr"
      :showToastMsg="showToastMsg"
      :showDialogMsg="showDialogMsg"
      :showConfirm="showConfirm"
    />
  </div>
</template>
<script>
/*eslint-disable */

import axios from "axios";
import EmployeeDetail from "./EmployeeDetail.vue";
import $ from "jquery";

export default {
  name: "employee-list",
  components: {
    EmployeeDetail,
  },
  methods: {
    // Hàm render ra Giới tính
    // 0 - nam
    // 1 - nữ
    // 2 - khác
    renderGender(gender) {
      switch (gender) {
        case 0:
          return "Nam";
        case 1:
          return "Nữ";
        case 2:
          return "Khác";
        default:
          return "Chưa có";
      }
    },

    // hàm load lại data mỗi thi crud nhân viên
    reloadData() {
      var me = this;
      // lấy dự liệu danh sách nhiên viên
      axios
        .get("http://amis.manhnv.net/api/v1/Employees")
        .then(function (res) {
          me.employees = res.data;
        })
        .catch(function (err) {
          // kiểm tra mã lỗi của api và thực hiện thao tác với các mã lỗi đó
          me.showToastMsgErr(err.response.data.Message);
        });
    },
    getname() {
      return this.Name;
    },
    /**
     * Mô tả : THực hiện hiện thị form thêm nhân viên
     * @param
     * @return
     * Created by: Cao Thanh Lâm - MF1103
     * Created date: 22:12 29/03/2022
     */
    btnAddOnClick() {
      try {
        let me = this;
        // gán formmode bằng add
        me.formMode = this.MISAEnum.FormMode.Add;
        this.showOrHideDialog(true);
        this.employeeSelected = {};
        // lây thông tin mã nhân viên mới
        axios
          .get("http://amis.manhnv.net/api/v1/Employees/NewEmployeeCode")
          .then((res) => {
            me.employeeSelected.EmployeeCode = res.data;
            // focus vào ô nhâp liệu đầu tiên
            document.getElementById("txtEmployeeCode").focus();
          })
          .catch((err) => {
            me.showToastMsgErr(err);
          });
      } catch (error) {
        me.showToastMsgErr(error);
      }
    },
    // Hàm show or hide dialog
    // param: isShow: true: hiện dialog, false: ẩn dialog
    showOrHideDialog(isShow) {
      this.isShowDialog = isShow;
    },
    /**
     * Mô tả : Hiển thị form cho phép sửa thông tin nhân viên
     * @param
     * @return
     * Created by: Cao Thanh Lâm - MF1103
     * Created date: 23:49 29/03/2022
     */
    rowOnDblClick(emp) {
      var me = this;
      me.formMode = this.MISAEnum.FormMode.Edit;
      this.employeeSelected = emp;
      // lấy thông tin nhân viên
      // C1 lây thôn tin nhân viên có sẵn dưới clients
      // C2 lấy thông tin nhân viên từ Api

      // binding lên form chi tiết ----> việc ủa component con

      // hiển thi form
      this.showOrHideDialog(true);
    },
    rowOnClick(e, emp) {
      // lấy các thẻ tr
      document.getElementById("btnDelete").style.display = "inline-block";
      let trElements = $("table#tblEmployeeList tbody tr");
      // nếu thẻ tr đã chứa class m-row-selected thì click vào hủy bỏ chọn
      if ($(e.target.parentElement).hasClass("m-row-selected")) {
        document.getElementById("btnDelete").style.display = "none";
        trElements.removeClass("m-row-selected");
        this.employeeSelected = {};
      } else {
        // remove hết các thẻ chứa m-row-selected
        trElements.removeClass("m-row-selected");
        this.employeeSelected = emp;
        // gán class m-row-selected cho thẻ tr được click
        e.target.parentElement.classList.add("m-row-selected");
      }
    },
    ComfirmDeleteEmployee() {
      let me = this;
      let employeeSelected = me.employeeSelected;
        // comfirm delete
        this.showConfirm(
          `Bạn có chắc chắn muốn xóa nhân viên &nbsp <b style="font-size:13px">${employeeSelected.EmployeeName}</b> `,
          function () {
            axios
              .delete(
                `http://amis.manhnv.net/api/v1/Employees/${employeeSelected.EmployeeId}`
              )
              // show toast message khi xóa thành công
              .then(function () {
                // show toast msg
                me.showToastMsg("Xóa thành công");
                // load lai dữ liệu sau khi xóa
                me.reloadData();
              })
              // show toast error
              .catch(function (err) {
                me.showToastMsgErr(err.response.data.devMsg);
              });
          }
        );
    },

    /**
     * Mô tả : Hàm định dạng ngày tháng
     * @param
     * @return
     * Created by: Cao Thanh Lâm - MF1103
     * Created date: 22:05 29/03/2022
     */
    // Formate ngày và hiện thị trên màn hình
    formatDate(value) {
      try {
        // nếu dữ liệu tồn tại
        if (value) {
          // lấy dữ liệu date từ trên api
          value = new Date(value);
          // lây ra ngày
          let date = value.getDate();
          // lấy tháng
          let month = value.getMonth() + 1;
          // lấy năm
          let year = value.getFullYear();
          // thêm số 0 vào đằng trước
          date = date < 10 ? `0${date}` : date;
          month = month < 10 ? `0${month}` : month;
          value = `${date}/${month}/${year}`;
        } else {
          value = "";
        }
        return value;
      } catch (error) {
        // show toast error nếu lỗi
        this.showToastMsgErr(error);
      }
    },
    // format tiền
    formatMoney(value) {
      try {
        if (value) {
          value = value.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1.");
        } else {
          value = "";
        }
        return value;
      } catch (error) {
        // show toast error
        this.showToastMsgErr(error);
      }
    },
  },

  data() {
    return {
      showToastMsgInfo: this.CommonJs.showToastMsgInfo,
      showToastMsgErr: this.CommonJs.showToastMsgErr,
      showToastMsg: this.CommonJs.showToastMsg,
      showDialogMsg: this.CommonJs.showDialogMsg,
      showConfirm: this.CommonJs.showConfirm,
      number: 0,
      formMode: this.MISAEnum.FormMode.Add,
      Name: "Cao Thanh Lâm",
      newEmployeeCode: null,
      employee: {
        // create ramdom employee
        fullName: "employee-list",
        id: "employee-list",
      },
      employees: [],
      isShowDialog: false,
      employeeSelected: {},
    };
  },
  // 1.before create
  beforeCreate() {},

  // 2created
  created() {
    var me = this;
    // lấy dự liệu danh sách nhiên viên
    axios
      .get("http://amis.manhnv.net/api/v1/Employees")
      .then(function (res) {
        me.employees = res.data;
      })
      .catch(function (err) {
        // kiểm tra mã lỗi của api và thực hiện thao tác với các mã lỗi đó
        me.showToastMsgErr(err.response.data.devMsg);
      });
  },

  // 3before mount
  beforeMount() {},

  // 4mounted
  mounted() {},

  // 5before update
  beforeUpdate() {},

  // 6updated
  updated() {},

  // 7before destroy
  beforeUnmount() {},

  // 8destroyed
  unmounted() {},
  // props
  props: [],
  /**
   * Các phương thức
   */
};
</script>
