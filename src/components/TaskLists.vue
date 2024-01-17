<script setup>
import { ref } from 'vue'
const tasks = ref([
    {name: "Task 1",time: 30},
    {name: "Task 2",time: 40},
    {name: "Task 3",time: 60},
    {name: "Task 4",time: 45},
    {name: "Task 5",time: 50},
]);

const showPopup = ref(false);
const editedTask = ref({ name: '', time: 0 });
const editedTaskIndex = ref(null);

const editTask = (index) => {
    editedTask.value = { ...tasks.value[index] };

    editedTaskIndex.value = index;
    showPopup.value = true;
}

const updateTask = () => {
    if (editedTaskIndex.value !== null) {
        tasks.value[editedTaskIndex.value] = { ...editedTask.value };
        showPopup.value = false;
    }
};

const closePopup = () => {
    showPopup.value = false;
};
</script>

<template>
    <div id="app" class="container mx-auto p-4">
        <h1 class="text-[28px] font-semibold mb-3">Task List</h1>

        <ul>
            <li v-for="(task, index) in tasks" :key="index" class="mb-2 border-2 border-blue-600 hover:border-green-600 rounded-[16px] p-3 min-w-[400px] flex justify-between">
                <div>{{ task.name }} - {{ task.time }} minutes </div>
                <button @click="editTask(index)" class="ml-2 bg-red-500 text-white px-2 py-1 rounded">Edit</button>
            </li>
        </ul>

        <div v-if="showPopup" class="popup bg-white p-4 mt-4 shadow">
            <div @click="closePopup" class="absolute top-2 right-2 bg-red-500 text-[#FFF] p-1 rounded-[4px] text-[12px] cursor-pointer">Close</div>
            <form @submit.prevent="updateTask">

                <div class="mb-3">
                    <label for="taskName" class="block text-[15px] pb-1 text-gray-70 text-left">Task Name:</label>
                    <input type="text" id="taskName" v-model="editedTask.name" required
                        class="mt-1 px-3 py-2 border-2 border-[#dddddd] focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-[8px]">
                </div>

                <div>
                    <label for="taskTime" class="block text-[15px] pb-1 text-gray-70 text-left">Time (minutes):</label>
                    <input type="number" id="taskTime" v-model.number="editedTask.time" required
                        class="mt-1 px-3 py-2 border-2 border-[#dddddd] focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-[8px]">
                </div>

                <button type="submit" class="mt-4 bg-green-500 text-white px-4 py-2 rounded">Update Task</button>
            </form>
        </div>

    </div>
</template>

<style scoped>
.popup {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: white;
    padding:50px 20px;
    border: 1px solid #ccc;
}
</style>

