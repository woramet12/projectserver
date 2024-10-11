<template>
    <div>
        <h1>ข้อมูลสนาม</h1>
        <div v-if="blog.thumbnail && blog.thumbnail !== 'null'" class="thumbnail-container">
            <img :src="BASE_URL + blog.thumbnail" alt="รูปภาพ thumbnail" class="thumbnail" />
        </div>
        <p>สนามที่: {{ blog.id }}</p>
        <p>ชื่อทีม: {{ blog.title }}</p>
        <p>ชื่อสนาม: {{ blog.content }}</p>
        <p>จำนวนความจุผู้เข้าชม: {{ blog.category }} ที่นัง</p>
        <p>ที่อยู่ของสนาม: {{ blog.address }}</p>
        <p>สถานะ: {{ blog.status }}</p>
        <p>วันที่สร้าง: {{ formatDate(blog.createdAt) }}</p>
        <p>แก้ไขล่าสุด: {{ formatDate(blog.updatedAt) }}</p>
        <div class="button-group">
            <button v-on:click="navigateTo('/blog/edit/' + blog.id)">แก้ไขข้อมูลสนาม</button>
            <button v-on:click="deleteBlog">ลบข้อมูลสนาม</button>
            <button v-on:click="navigateTo('/blogs')">กลับ</button>
        </div>
    </div>
</template>

<script>
    import BlogsService from '@/services/BlogsService'
    export default {
        data () {
            return {
                BASE_URL: "http://localhost:8081/assets/uploads/",
                blog: null
            }
        },
        async created () {
            try {
                let blogId = this.$route.params.blogId
                this.blog = (await BlogsService.show(blogId)).data
            } catch (error) {
                console.log(error)
            }
        },
        methods : {
            navigateTo (route) {
                this.$router.push(route)
            },
            async deleteBlog() {
                if (confirm("คุณแน่ใจหรือว่าต้องการลบข้อมูลสนามนี้?")) {
                    try {
                        await BlogsService.delete(this.blog.id)
                        alert("ลบข้อมูลสนามเรียบร้อยแล้ว")
                        this.navigateTo('/blogs')
                    } catch (error) {
                        console.log(error)
                        alert("เกิดข้อผิดพลาดในการลบข้อมูลสนาม")
                    }
                }
            },
            formatDate(date) {
                if (!date) return '-'
                return new Date(date).toLocaleDateString()
            }
        }
    }
</script>

<style scoped>
.thumbnail-container {
    text-align: center;
    margin-bottom: 20px;
}
.thumbnail {
    max-width: 50%;
    height: auto;
    border-radius: 8px;
}
.button-group {
    display: flex;
    gap: 10px;
    margin-top: 20px;
}
button {
    padding: 10px 20px;
    border: none;
    background-color: #4CAF50;
    color: white;
    border-radius: 5px;
    cursor: pointer;
}
button:hover {
    background-color: #45a049;
}
</style>
