<script>
import BarChart from '../components/chart/BarChart.vue'
import PieChart from '../components/chart/PieChart.vue'
import TaskTable from '../components/TaskTable.vue'
import UserProjectTable from '../components/UserProjectTable.vue'
import { useProjects } from '@/composables/useProjects'
import { ref, watch, onMounted } from 'vue'

const { projects, fetchProjects } = useProjects()

onMounted(() => {
    console.log('Component mounted, fetching projects...')
    fetchProjects()
})

export default {
    components: {
        BarChart,
        PieChart,
        TaskTable,
        UserProjectTable
    }
}
</script>
<template>
    <div class="mypage-wrap">
        <div class="top-area">
            <div class="inner">
                <div class="profile">
                    <i class=""></i>
                    <div class="info">
                        <p class="info-text">부서명</p>
                        <h3 class="info-name">홍길동</h3>
                    </div>
                </div>
            </div>
        </div>
        <div class="btm-area">
            <div class="inner">
                <div class="row align-items-center justify-content-between g-3 pb-4">
                    <div class="col-auto">
                        <div class="title-area">
                            <h2 class="h2">[개인통계]마이페이지 작업</h2>
                            <p class="text-body-tertiary lh-sm mb-3">testesttextext</p>
                        </div>
                    </div>
                </div>

                <div class="row mb-5">
                    <div class="col-xl-6">
                        <h3 class="h3">나의 업무 진행 상태 분포</h3>
                        <BarChart />
                    </div>
                    <div class="col-xl-6">
                        <h3 class="h3">나의 우선순위별 업무 분포</h3>
                        <BarChart />
                    </div>
                </div>

                <div class="row mb-5">
                    <div class="col">
                        <h3 class="h3">나의 주간 참여 프로젝트 분포</h3>
                        <PieChart />
                    </div>
                </div>

                <div class="row mb-5">
                    <div class="col">
                        <h3 class="h3">나의 전체 프로젝트 목록</h3>
                        <UserProjectTable :projects="projects" />
                    </div>
                </div>

                <div class="row mb-5">
                    <div class="col-xl-6">
                        <h3 class="h3">나의 전체 업무 목록</h3>
                        <TaskTable />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.top-area {
    position: relative;
    height: 280px;
    background-color: #dbf6eb;
    background-image: url('../public/images/img-mypage.svg');
    background-position: 100% 140%;
    background-repeat: no-repeat;
    background-size: 800px;
}
.profile {
    display: flex;
    align-items: flex-end;
    justify-content: start;
}
.profile i {
    display: inline-block;
    width: 160px;
    height: 160px;
    margin-top: 160px;
    margin-left: 60px;
    border-radius: 100%;
    border: 3px solid #fff;
    background-color: #d9d9d9;
}

.profile .info {
    padding-bottom: 22px;
    padding-left: 20px;
}

.profile .info-text {
    margin-bottom: 5px;
    font-size: 16px;
    font-weight: 300;
}
.profile .info-name {
    font-size: 36px;
    font-weight: bold;
}
.btm-area {
    padding-top: 120px;
}
</style>
