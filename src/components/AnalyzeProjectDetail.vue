<script>
import UserProfile from '../components/UserProfile.vue'
import ProgressBar from '../components/ProgressBar.vue'
import StatusBadge from '../components/StatusBadge.vue'
import PriorityBadge from '../components/PriorityBadge.vue'
import { formatDate } from '@/utils/dateUtils.js'

export default {
    components: {
        UserProfile,
        ProgressBar,
        StatusBadge,
        PriorityBadge,
        formatDate
    },
    props: {
        project: {
            type: Object,
            required: true
        }
    },
    setup() {
        return {
            formatDate
        }
    }
}
</script>
<template>
    <div v-if="project && project.projTitle" class="row align-items-center justify-content-between g-3 pb-4">
        <div class="col-auto">
            <div class="title-area">
                <p class="text-body-tertiary lh-sm mb-3">ProjectNo.{{ project.projNo }}</p>
                <h2 class="h2">📊[통계분석] {{ project.projTitle }}</h2>
                <div class="text-area">{{ project.projDesc }}</div>
            </div>
        </div>
    </div>

    <table class="table table-borderless fs-9 mb-5 border-top border-translucent">
        <colgroup>
            <col style="width: 154px" />
            <col />
            <col style="width: 154px" />
            <col />
        </colgroup>
        <tbody>
            <tr>
                <th>프로젝트 기간</th>
                <td>
                    {{ formatDate(project.projStartDate) }} ~ {{ formatDate(project.projEndDate) }}
                    <StatusBadge :status="project.projectStatus" />
                </td>
            </tr>
            <tr>
                <th>프로젝트 참여자</th>
                <td>
                    참여자 수정해야해수정해야해
                    <!-- <div v-for="(participant, index) in formatParticipants(project.participants).visibleParticipants" :key="index">
                        <UserProfile :name="participant" />
                    </div>
                    <span v-if="formatParticipants(project.participants).overflowCount > 0">...</span> -->
                </td>
            </tr>
            <tr>
                <th>진행률</th>
                <td><ProgressBar :progress="project.projPercent" /></td>
                <th></th>
                <td></td>
            </tr>
            <tr>
                <th>작성일</th>
                <td>{{ formatDate(project.projCreateDate) }}</td>
            </tr>
        </tbody>
    </table>
</template>
<style scoped>
.table-borderless {
    border-top: 2px solid #e6eef4;
}

.table-borderless th,
.table-borderless td {
    padding: 15px 0;
    font-size: 14px !important;
    text-align: left;
}
.table-borderless th {
    border-bottom: 0;
    color: #384554;
    font-weight: 700;
}

.table-borderless td {
    border-bottom: 0 !important;
    color: #656f7d;
}
.table-borderless th:nth-child(3) {
    padding-left: 30px;
}

.dash-line {
    margin: 0;
    border: 0;
    border-top: 1px dashed #d2dce5;
    opacity: 0.6;
}
.text-area {
    padding: 30px;
    padding-left: 0;
    font-size: 14px;
    color: #333;
    line-height: 1.8;
}
</style>
