<template>
    <div class="m-navbar">
        <router-link  to="/employee" class="m-menu-item m-menu-selected">
            <div class="m-icon-item"><i class="fa-solid fa-user-group"></i></div>
            <div class="m-text-item">Danh sach nhan vien</div>
        </router-link>
        <router-link to="/customer" class="m-menu-item">
            <div class="m-icon-item"><i class="fa-solid fa-list-ol"></i></div>
            <div class="m-text-item">Danh sach khach hang</div>
        </router-link>
        <router-link to="/report" class="m-menu-item">
            <div class="m-icon-item"><i class="fa-regular fa-file"></i></div>
            <div class="m-text-item">Bao cao</div>
        </router-link>
        <router-link to="/setting" class="m-menu-item">
            <div class="m-icon-item"><i class="fa-solid fa-gear"></i></div>
            <div class="m-text-item">Cai dat</div>
        </router-link>
    </div>
</template>
<script>
export default {
  name: "TheNavbar",
};
</script>
<style scoped>
@import url("../../css/layout/navbar.css");
</style>