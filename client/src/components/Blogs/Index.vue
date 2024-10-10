<template>
    <div>
        <h2>Get all blogs</h2>
        <h4>จำนวน blog {{blogs.length}}</h4>
        <p><button v-on:click="navigateTo('/blog/create')">สร้าง blog</button></p>
        <div class="content-grid">
            <div v-for="blog in blogs" v-bind:key="blog.id" class="blog-card">
                <p>id: {{ blog.id }}</p>
                <p>title: {{ blog.title }}</p>
                <p>content: {{ blog.content }}</p>
                <p>category: {{ blog.category }}</p>
                <p>status: {{ blog.status }}</p>
                <p>
                    <button v-on:click="navigateTo('/blog/' + blog.id)">ดู blog</button>
                    <button v-on:click="navigateTo('/blog/edit/' + blog.id)">แก้ไข blog</button>
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
    padding: 10px; /* ลด padding เพื่อให้ข้อความใกล้ขอบมากขึ้น */
    
    color: #f1c40f;
    box-sizing: border-box;
}

/* สำหรับหน้าจอขนาดเล็ก ให้แสดงการ์ดเป็นคอลัมน์เดียว */
@media (max-width: 768px) {
    .content-grid {
        grid-template-columns: 1fr;
    }
}
</style>
