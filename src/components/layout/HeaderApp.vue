<script>
import UserProfile from '@/components/UserProfile.vue'

export default {
    components: {
        UserProfile
    },
    data() {
        return {
            userInfo: null,
            name: '',
            email: ''
        }
    },
    mounted() {
        this.userInfo = this.$cookies.get('user')
        console.log(this.userInfo)
        if (this.userInfo) {
            this.name = this.userInfo.name
            this.email = this.userInfo.email
        }
    }
}
</script>
<template>
    <header>
        <div class="inner">
            <div class="d-flex align-items-center justify-content-between">
                <h1 class="logo">
                    <a href="/dashboard"><img src="/public/images/logo.svg" alt="boogle logo" /></a>
                </h1>

                <nav class="navbar navbar-expand-lg">
                    <div class="collapse navbar-collapse" id="navbarSupportedContent">
                        <!-- 로그인한 상태에서 보여줄 탭 -->
                        <ul class="navbar-nav me-auto mb-2 mb-lg-0" v-if="userInfo">
                            <li class="nav-item">
                                <router-link to="/project" class="nav-link">프로젝트</router-link>
                            </li>
                            <li class="nav-item">
                                <router-link to="/analyze" class="nav-link">analyze</router-link>
                            </li>
                            <li>
                                <div class="vertical-line"></div>
                            </li>
                            <li class="nav-item">
                                <router-link to="/search" class="nav-link">
                                    <button class="btn" type="submit"><i class="bi bi-search"></i></button>
                                </router-link>
                            </li>
                        </ul>

                        <ul>
                            <li>
                                <router-link to="/mypage" class="nav-link">mypage 개인분석</router-link>
                            </li>
                        </ul>

                        <!-- 로그인한 상태에서 보여줄 UserProfile 드롭다운 -->
                        <ul class="me-auto mb-2 mb-lg-0" v-if="userInfo">
                            <li class="dropdown">
                                <UserProfile :name="userInfo.name" :dept="userInfo.dept" />
                            </li>
                        </ul>
                    </div>
                </nav>
            </div>
        </div>
    </header>
</template>

<style>
.vertical-line {
    border-left: 1px solid #eaeaea;
    height: 40px;
    align-self: center;
    margin: 0 25px;
}

.navbar-nav.me-auto.mb-2.mb-lg-0 .nav-item:not(:last-child) {
    margin-right: 20px;
    align-self: center;
}
</style>
