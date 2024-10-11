<template>
    <div>
        <h2>สนามฟุตบอลพรีเมียร์ลีค</h2>
        <h4>จำนวนสนาม {{ blogs.length }}</h4>
        <p><button v-on:click="navigateTo('/blog/create')">เพิ่มสนาม</button></p>
        <div class="content-grid">
            <div v-for="blog in blogs" v-bind:key="blog.id" class="blog-card">
                <div v-if="blog.thumbnail && blog.thumbnail !== 'null'" class="thumbnail-container">
                    <img :src="BASE_URL + blog.thumbnail" alt="รูปภาพ thumbnail" class="thumbnail" />
                </div>
                <p>สนามที่: {{ blog.id }}</p>
                <p>ชื่อทีม: {{ blog.title }}</p>
                <p>ชื่อสนาม: {{ blog.content }}</p>
                <p>จำนวนความจุผู้เข้าชม: {{ blog.category }} คน</p>
                <p>ที่อยู่ของสนาม: {{ blog.address }}</p>
                <p>สถานะ: {{ blog.status }}</p>
                <p>
                    <button v-on:click="navigateTo('/blog/' + blog.id)">ดูข้อมูล</button>
                    <button v-on:click="navigateTo('/blog/edit/' + blog.id)">แก้ไข</button>
                    <button v-on:click="deleteBlog(blog)">ลบข้อมูล</button>
                </p>
            </div>
        </div>
        <p><button v-on:click="logout">Logout</button></p>
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
                this.$router.push({
                    name: 'login'
                })
            },
            navigateTo(route) {
                this.$router.push(route)
            },
            async deleteBlog(blog) {
                let result = confirm("Want to delete?")
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
    background-color: #360852;
    border-radius: 10px;
    padding: 10px;
    color: #f1c40f;
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

/* สำหรับหน้าจอขนาดเล็ก ให้แสดงการ์ดเป็นคอลัมน์เดียว */
@media (max-width: 768px) {
    .content-grid {
        grid-template-columns: 1fr;
    }
}
</style>
