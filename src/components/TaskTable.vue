<script setup>
import { ref, defineProps, onMounted, watch } from 'vue'
import axios from 'axios'
import { useRoute } from 'vue-router'
import { formatDate } from '@/utils/dateUtils.js'

import UserProfile from '../components/UserProfile.vue'
import ProgressBar from '../components/ProgressBar.vue'
import StatusBadge from '../components/StatusBadge.vue'
import PriorityBadge from '../components/PriorityBadge.vue'
import TaskDetailModal from '../components/TaskDetailModal.vue'

const props = defineProps({
    initialTasks: Array,
    newTaskData: Object,
    addNewTask: Function,
    projectId: Number,
    tasks: Array
})

const tasks = ref(props.initialTasks)
const route = useRoute()
const isModalActive = ref(false)
const selectedTask = ref(null)
const error = ref(null)

async function fetchProjectTasks() {
    const projectId = route.params.id || props.projectId

    try {
        const apiUrl = import.meta.env.VITE_API_URL
        const response = await axios.get(`${apiUrl}/task/project/${projectId}/task`)
        tasks.value = response.data
        error.value = null
    } catch (error) {
        console.error('프로젝트 태스크 데이터를 가져오는데 실패했습니다:', error)
        error.value = '프로젝트 태스크 데이터를 가져오는데 실패했습니다.'
        tasks.value = [] // 데이터를 비워 테이블을 숨깁니다.
    }
}

watch(
    () => props.newTaskData,
    (newVal) => {
        if (newVal) {
            tasks.value.push(newVal)
            tasks.value = [...tasks.value]
        }
    }
)

onMounted(() => {
    if (!props.initialTasks || props.initialTasks.length === 0) {
        fetchProjectTasks()
    }
})

const openModal = (task) => {
    selectedTask.value = task
    isModalActive.value = true
}

// 초기 데이터 또는 태스크 변경 감시
watch(
    () => props.initialTasks,
    () => {
        fetchProjectTasks()
    }
)

watch(
    () => props.tasks,
    () => {
        tasks.value = props.tasks
    }
)
</script>

<template>
    <div v-if="error">{{ error }}</div>
    <table class="table fs-9 mb-5 border-top border-translucent">
        <colgroup>
            <col />
            <col style="width: 80px" />
            <col style="width: 100px" />
            <col style="width: 100px" />
            <col style="width: 100px" />
            <col style="width: 180px" />
            <col style="width: 80px" />
            <col style="width: 50px" />
            <col style="width: 100px" />
        </colgroup>
        <thead>
            <tr>
                <th class="sort white-space-nowrap align-middle" scope="col" data-sort="project_title">업무명</th>
                <th class="sort align-middle" scope="col" data-sort="">담당자</th>
                <th class="sort align-middle" scope="col" data-sort="start_date">시작일</th>
                <th class="sort align-middle" scope="col" data-sort="end_date">종료일</th>
                <th class="sort text-start ps-5 align-middle" scope="col" data-sort="status">진행상태</th>
                <th class="sort text-center align-middle" scope="col" data-sort="progress">진행률</th>
                <th class="sort text-end align-middle" scope="col" data-sort="priority">우선순위</th>
                <th class="sort text-end align-middle" scope="col" data-sort="test">TEST</th>
                <th class="sort text-end pe-0 align-middle" scope="write_date">작성일</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(task, index) in tasks" :key="index">
                <td>
                    <a class="tb-project-title" @click="openModal(task)">{{ task.task_title }}</a>
                </td>
                <td class="text-start">
                    <UserProfile :name="task.assignee" />
                </td>
                <td>{{ formatDate(task.start_date) }}</td>
                <td>{{ formatDate(task.end_date) }}</td>
                <td><StatusBadge :status="task.task_status" /></td>
                <td class="text-end"><ProgressBar :progress="task.task_percent" /></td>
                <td class="text-end"><PriorityBadge :priority="task.task_priority" /></td>
                <td class="text-end text-secondary">{{ task.task_test }}</td>
                <td class="text-end text-secondary" style="font-size: 12px">{{ formatDate(task.create_date) }}</td>
            </tr>

            <!-- 새로운 업무 표시 -->
            <tr v-if="newTaskData" :key="newTaskData.id">
                <td>
                    <a class="tb-project-title" @click="openModal(newTaskData)">{{ newTaskData.task_title }}</a>
                </td>
                <td>{{ newTaskData.assignee }}</td>
                <td>{{ formatDate(newTaskData.start_date) }}</td>
                <td>{{ formatDate(newTaskData.end_date) }}</td>
                <td>{{ newTaskData.task_status }}</td>
                <td>{{ newTaskData.task_percent }}</td>
                <td>{{ newTaskData.task_priority }}</td>
                <td>{{ newTaskData.task_test }}</td>
                <td>{{ formatDate(newTaskData.create_date) }}</td>
            </tr>
        </tbody>
    </table>

    <TaskDetailModal :is-active="isModalActive" :task="selectedTask" @close-modal="isModalActive = false" @refreshTasks="fetchProjectTasks" />
</template>

<style scoped>
.tb-project-title {
    position: relative;
    padding-left: 15px;
    cursor: pointer;
}
.tb-project-title::after {
    display: block;
    content: 'ㄴ';
    position: absolute;
    top: 0;
    left: 0;
    opacity: 0.7;
    font-weight: 300;
}
</style>
