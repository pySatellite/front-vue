<script>
import axios from 'axios'
import globalInfo from '@/utils/globalInfoUtils.js'

export default {
    data() {
        return {
            username: '',
            password: '',
            errorMessage: ''
        }
    },
    methods: {
        login() {
            let url = import.meta.env.VITE_API_URL + '/signin'
            console.log(this.username + ' / ' + this.password)
            let data = {
                email: this.username,
                password: this.password
            }
            axios
                .post(url, data, {
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Credentials': true
                })
                .then((res) => {
                    if (res.status == 200) {
                        const userInfo = {
                            name: res.data.name,
                            email: res.data.email
                        }
                        this.$cookies.set('user', userInfo)
                        this.setLoginInfo(res.data)
                        this.$router.push('/dashboard').then(() => {
                            location.reload()
                        })
                    } else {
                        console.log(res.status)
                    }
                })
                .catch((error) => {
                    console.log(error)
                })
        },
        setLoginInfo(user) {
            globalInfo.UserInfo.name = user.name
            globalInfo.UserInfo.eno = user.eno
            globalInfo.UserInfo.dept = user.dept_no
            globalInfo.UserInfo.position = user.position_id
        }
    }
}
</script>

<template>
    <div class="login-page">
        <div class="inner">
            <div class="card">
                <div class="card-body">
                    <div class="title-area">
                        <h2 class="h3">로그인</h2>
                        <p class="h5">로그인을 해주세요.</p>
                    </div>
                    <form @submit.prevent="login" class="mt-3">
                        <div class="mb-3">
                            <input type="email" id="username" v-model="username" placeholder="example@boogle.com" class="form-control" />
                        </div>
                        <div class="mb-3">
                            <input type="password" id="password" v-model="password" placeholder="비밀번호를 입력해주세요" class="form-control" />
                        </div>
                        <button type="submit" class="btn btn-primary">Login</button>
                    </form>
                    <p v-if="errorMessage" class="mt-3 text-danger">{{ errorMessage }}</p>
                </div>
            </div>
        </div>
    </div>
</template>
<style scoped>
.login-page {
    background-color: #f5f5f5;
}
.card {
    width: 850px;
    margin: 0 auto;
}
</style>
