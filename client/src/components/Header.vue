<template>
  <div>
    <div class="nv-navbar">
      <!-- ส่วนของโลโก้ -->
      <img src="https://upload.wikimedia.org/wikipedia/th/thumb/1/12/PL_Logo.svg/1200px-PL_Logo.svg.png" alt="โลโก้" class="nv-logo" />
      <ul class="nav">
        <li><router-link :to="{ name: 'blogs' }">Add a Football Field</router-link></li>
        <li><router-link :to="{ name: 'users' }">Users</router-link></li>
        <!-- ตรวจสอบสถานะการเข้าสู่ระบบ -->
        <li v-if="isLogin()">
          <a @click="logout">Logout</a>
        </li>
        <li v-else>
          <router-link :to="{ name: 'login' }">Login</router-link>
        </li>
      </ul>
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
  background: linear-gradient(90deg, #ffe066, #ffd54f); /* ไล่สีจากซ้ายไปขวา */
  width: 95%;
  padding: 10px 20px;
  display: flex;
  align-items: center;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2);
  border-radius: 8px;
  margin: 0 auto;
}

/* สไตล์โลโก้ */
.nv-logo {
  height: 45px;
  margin-right: 25px;
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
  margin-left: 20px;
}

/* สไตล์ลิงก์ในเมนู */
.nv-navbar .nav li a {
  padding: 10px 15px;
  text-decoration: none;
  color: #333;
  font-weight: bold;
  border-radius: 5px;
  transition: background 0.3s, color 0.3s, transform 0.3s;
}

/* เอฟเฟกต์ hover */
.nv-navbar .nav li a:hover {
  background-color: rgba(255, 255, 255, 0.4);
  color: #000;
  transform: scale(1.05);
}

/* สไตล์สำหรับลิงก์ที่ active */
.nv-navbar .nav li a.router-link-active {
  background-color: #ffd54f;
  color: #000;
}
</style>
