<script setup>
import { ref } from "vue";
import Modal from '@/components/Modal.vue'

const showModal = ref(false);
const tableData = ref([]);

const toggleModal = () => {
    showModal.value != showModal.value;
}

const form = ({
    name:'',
    email:''
})

const submitForm = () => {
    tableData.value.push({...form.value})
    form.value = {
        name:'',
        email:''
    }
}
</script>

<template>
    <div>
        <table>
            <tr>
                <th>
                    name
                </th>
                <th>
                    email
                </th>
            </tr>

            <tr v-for="(credential , index) in tableData" :key="index">
                <td>
                    {{ credential.name }}
                </td>

                <td>
                    {{ credential.email }}
                </td>
            </tr>
        </table>
    </div>
    <teleport>
        <Modal v-if="showModal" @close="toggleModal">
            <form @submit.prevent="submitForm" method="post">
            <label for="name">Name</label>
            <input type="text" id="name" v-model="form.name" required>

            <label for="email">Email</label>
            <input type="email" id="email" v-model="form.email" required>

            <button type="submit">Submit</button>
        </form>
        </Modal>

    </teleport>

    <!-- Modal backdrop -->
    <div class="fixed inset-0 z-50 flex items-center justify-center bg-gray-800 bg-opacity-50">
        <div class="bg-white ">

        </div>
    </div>
</template>