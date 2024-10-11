<template>
    <div>
        <h2>สนามฟุตบอลพรีเมียร์ลีค</h2>
        <h4>จำนวนสนาม {{ blogs.length }}</h4>
        <p><button v-on:click="navigateTo('/blog/create')" class="action-button">เพิ่มสนาม</button></p>
        <div class="content-grid">
            <div v-for="blog in blogs" v-bind:key="blog.id" class="blog-card">
                <div v-if="blog.thumbnail && blog.thumbnail !== 'null'" class="thumbnail-container">
                    <img :src="BASE_URL + blog.thumbnail" alt="รูปภาพ thumbnail" class="thumbnail" />
                </div>
                <p>สนามที่: {{ blog.id }}</p>
                <p>ชื่อทีม: {{ blog.title }}</p>
                <p>ชื่อสนาม: {{ blog.content }}</p>
                <p>จำนวนความจุผู้เข้าชม: {{ blog.category }} ที่นั่ง</p>
                <p>ที่อยู่ของสนาม: {{ blog.address }}</p>
                <p>สถานะ: {{ blog.status }}</p>
                <p>
                    <button v-on:click="navigateTo('/blog/' + blog.id)" class="action-button">ดูข้อมูล</button>
                    <button v-on:click="navigateTo('/blog/edit/' + blog.id)" class="action-button">แก้ไข</button>
                    <button v-on:click="deleteBlog(blog)" class="action-button delete-button">ลบข้อมูล</button>
                </p>
            </div>
        </div>
        <p><button v-on:click="logout" class="action-button">Logout</button></p>
    </div>
</template>

<script>
    import BlogsService from '@/services/BlogsService'
    export default {
        data() {
            return {
                BASE_URL: "http://localhost:8081/assets/uploads/",
                blogs: []
            }
        },
        async created() {
            this.blogs = (await BlogsService.index()).data
        },
        methods: {
            logout() {
                this.$store.dispatch('setToken', null)
                this.$store.dispatch('setBlog', null)
                this.$router.push({ name: 'login' })
            },
            navigateTo(route) {
                this.$router.push(route)
            },
            async deleteBlog(blog) {
                let result = confirm("ต้องการลบข้อมูลหรือไม่?")
                if (result) {
                    try {
                        await BlogsService.delete(blog)
                        this.refreshData()
                    } catch (err) {
                        console.log(err)
                    }
                }
            },
            async refreshData() {
                this.blogs = (await BlogsService.index()).data
            }
        }
    }
</script>

<style scoped>
/* จัดการให้การ์ดแสดงเป็นตาราง 2x2 */
.content-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
}

/* สไตล์ของการ์ด */
.blog-card {
    background-color: #630c9a;
    border-radius: 10px;
    padding: 10px;
    color: #ffffff;
    box-sizing: border-box;
}

/* สไตล์สำหรับรูป thumbnail */
.thumbnail-container {
    text-align: center;
    margin-bottom: 10px;
}
.thumbnail {
    max-width: 50%;
    height: auto;
    border-radius: 5px;
}

/* สไตล์สำหรับปุ่ม */
.action-button {
    padding: 10px 20px;
    margin: 5px;
    border: none;
    background: linear-gradient(145deg, #6b0f84, #4e0871);
    color: #fff;
    border-radius: 8px;
    cursor: pointer;
    box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.3);
    transition: all 0.3s ease;
}

.action-button:hover {
    background: linear-gradient(145deg, #4e0871, #6b0f84);
    box-shadow: 0px 6px 8px rgba(0, 0, 0, 0.4);
}

.action-button:active {
    transform: scale(0.95);
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
}

/* สไตล์เพิ่มเติมสำหรับปุ่มลบ */
.delete-button:hover {
    background: linear-gradient(145deg, #d32f2f, #b71c1c);
}

/* สำหรับหน้าจอขนาดเล็ก ให้แสดงการ์ดเป็นคอลัมน์เดียว */
@media (max-width: 768px) {
    .content-grid {
        grid-template-columns: 1fr;
    }
}
</style>
