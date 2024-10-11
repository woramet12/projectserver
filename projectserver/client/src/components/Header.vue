<template>
  <div>
    <div class="nv-navbar">
      <!-- ส่วนของโลโก้ -->
      <img src="https://upload.wikimedia.org/wikipedia/th/thumb/1/12/PL_Logo.svg/1200px-PL_Logo.svg.png" alt="โลโก้" class="nv-logo" />
      <ul class="nav">
        <li><router-link :to="{ name: 'blogs' }">add a football field</router-link></li>  

        <li><router-link :to="{ name: 'comments' }">Comments</router-link></li>
        <li><router-link :to="{ name: 'users' }">Users</router-link></li>

        <li><router-link :to="{ name: 'comments' }">Comments</router-link></li>
        <li><router-link :to="{ name: 'users' }">Users</router-link></li>

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
  padding: 5px 2px; /* ลด padding ด้านซ้ายให้เหลือ 10px */
  display: flex;
  align-items: center;
}

/* สไตล์โลโก้ */
.nv-logo {
  height: 35px; /* ลดขนาดความสูงของโลโก้เล็กน้อย */
  margin-right: 15px;
}

/* สไตล์เมนูนำทาง */

/* ปรับสไตล์สำหรับแถบนำทาง */
.nv-navbar {
  background: linear-gradient(90deg, #ffe066, #ffd54f); /* การไล่สีจากซ้ายไปขวา */
  width: 95%;
  padding: 10px 20px;
  display: flex;
  align-items: center;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2); /* เพิ่มเงาเพื่อความลึก */
  border-radius: 8px; /* เพิ่มความโค้งมนให้แถบ */
}

/* ปรับสไตล์โลโก้ */
.nv-logo {
  height: 45px; /* เพิ่มขนาดโลโก้เล็กน้อย */
  margin-right: 25px;
}

/* ปรับสไตล์เมนูนำทาง */

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
/* ปรับสไตล์รายการเมนู */
.nv-navbar .nav li {
  margin-left: 20px; /* เพิ่มระยะห่างระหว่างเมนูให้มากขึ้น */
}

/* ปรับสไตล์ลิงก์ในเมนู */
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

  color: darkslategrey;
  color: darkslategrey;

  background-color: rgba(255, 255, 255, 0.4); /* เพิ่มพื้นหลังสีขาวโปร่งใส */
  color: #000;
  transform: scale(1.05); /* ขยายเล็กน้อยเมื่อวางเมาส์ */

}

/* สไตล์สำหรับลิงก์ที่ active */
.nv-navbar .nav li a.router-link-active {

  background-color: gold;
  color: darkslategrey;

  background-color: gold;
  color: darkslategrey;

  background-color: #ffd54f;
  color: #000;

}

/* ล้างการลอยขององค์ประกอบ */
.clearfix {

  clear: left;


  clear: left;

  clear: both;
}
</style>
