<template>
  <div>
    <div class="nv-navbar">
      <!-- ส่วนของโลโก้ -->
      <img src="https://upload.wikimedia.org/wikipedia/th/thumb/1/12/PL_Logo.svg/1200px-PL_Logo.svg.png" alt="โลโก้" class="nv-logo" />
      <ul class="nav">
        <li><router-link :to="{ name: 'blogs' }">add a football field</router-link></li>  
        <li><router-link :to="{ name: 'comments' }">Comments</router-link></li>
        <li><router-link :to="{ name: 'users' }">Users</router-link></li>
        <!-- ถ้า isLogin เป็นจริง ให้แสดงเมนูออกจากระบบ มิฉะนั้น ให้แสดงเมนูเข้าสู่ระบบ -->
        <li v-if="isLogin()">
          <a @click="logout">Logout</a>
        </li>
        <li v-else>
          <router-link :to="{ name: 'login' }">Login</router-link>
        </li>
      </ul>
      <div class="clearfix"></div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    isLogin() {
      return this.$store.getters.isUserLoggedIn;
    },
    logout() {
      this.$store.dispatch('logout');
      this.$router.push({ name: 'login' });
    }
  }
};
</script>

<style scoped>
/* สไตล์สำหรับแถบนำทาง */
.nv-navbar {
  background-color: palegoldenrod;
  width: 100%;
  padding: 5px 4px; /* ลด padding ด้านซ้ายให้เหลือ 10px */
  display: flex;
  align-items: center;
}

/* สไตล์โลโก้ */
.nv-logo {
  height: 35px; /* ลดขนาดความสูงของโลโก้เล็กน้อย */
  margin-right: 15px;
}

/* สไตล์เมนูนำทาง */
.nv-navbar .nav {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  align-items: center;
}

/* สไตล์รายการเมนู */
.nv-navbar .nav li {
  margin-left: 3px;
}

/* สไตล์ลิงก์ในเมนู */
.nv-navbar .nav li a {
  padding: 5px 10px; /* ลด padding เพื่อทำให้ลิงก์ดูสั้นลง */
  text-decoration: none;
  color: gray;
  font-weight: bold;
}

/* เอฟเฟกต์ hover */
.nv-navbar .nav li a:hover {
  color: darkslategrey;
}

/* สไตล์สำหรับลิงก์ที่ active */
.nv-navbar .nav li a.router-link-active {
  background-color: gold;
  color: darkslategrey;
}

/* ล้างการลอยขององค์ประกอบ */
.clearfix {
  clear: left;
}
</style>
