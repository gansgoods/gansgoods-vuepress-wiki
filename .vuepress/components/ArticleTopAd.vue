<template>
    <div>
        <button @click="incrementCounter">点击次数: {{ counter }}</button>
        <button @click="getbaidu()">请求公开api的的方法</button>
        <button @click="fetchCloudFunction()">请求云函数的方法</button>
        <!-- 时间显示卡片 -->
        <div class="time-display" v-if="cloudTime">
            <h3>云函数时间</h3>
            <p>{{ formattedTime }}</p>
        </div>
        <div class="user-list">
            <div class="user-card" v-for="user in users" :key="user.id">
                <img :src="user.avatar" :alt="user.first_name" class="user-avatar" />
                <div class="user-info">
                    <h2>{{ user.first_name }} {{ user.last_name }}</h2>
                    <p>{{ user.email }}</p>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            counter: 99,
            users: [], // 初始用户数据为空
            cloudTime: null, // 用于存储从云函数获取的时间
        };
    },
    computed: {
        // 格式化时间显示
        formattedTime() {
            if (!this.cloudTime) return '';
            // 假设返回的时间格式是 ISO 8601，使用 JavaScript 的 Date 对象进行格式化
            const date = new Date(this.cloudTime);
            return date.toLocaleString(); // 根据需要调整时间格式
        }
    },
    methods: {
        async getbaidu() { // 注意异步
            try {
                const response = await fetch('https://reqres.in/api/users/');
                const data = await response.json(); // 解析JSON数据
                this.users = data.data; // 将用户数据保存到users数组
            } catch (error) {
                console.error("请求用户数据失败:", error);
            }
        },
        incrementCounter() {
            this.counter += 1;
        },
        // 新增方法用于访问云函数
        async fetchCloudFunction() {
            try {
                // 使用正确的URL和查询参数
                const response = await fetch('https://fc-mp-76e7e9ef-6ceb-4633-9fab-0d63c711a900.next.bspapp.com/mytest');
                const data = await response.json(); // 解析JSON数据
                this.cloudTime = data.currentTime; // 存储时间
                console.log('云函数返回结果:', data); // 打印返回结果
            } catch (error) {
                console.error("请求云函数失败:", error);
            }
        },

    },
};
</script>

<style>
.user-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    padding: 20px;
}

.user-card {
    margin: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    width: 200px;
    overflow: hidden;
    border-radius: 8px;
    text-align: center;
    background: #fff;
}

.user-avatar {
    width: 100%;
    height: auto;
    border-bottom: 1px solid #eee;
}

.user-info {
    padding: 15px;
}

.user-info h2 {
    margin: 0;
    font-size: 18px;
    color: #333;
}

.user-info p {
    font-size: 14px;
    color: #666;
}

.time-display {
    margin: 20px 0;
    padding: 10px;
    background-color: #f0f0f0;
    border-radius: 8px;
    text-align: center;
}

.time-display h3 {
    margin: 0;
    color: #333;
}

.time-display p {
    margin: 5px 0 0;
    color: #666;
    font-size: 18px;
}
</style>
