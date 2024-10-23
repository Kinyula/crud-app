<script setup>
import { ref, onMounted } from "vue";
import Modal from "@/components/Modal.vue";
import { useToast } from "vue-toastification";

const toast = useToast();

// State for modal visibility
const showModal = ref(false);

// Function to toggle modal visibility
const toggleModal = () => {
  showModal.value = !showModal.value;
};

// State for form inputs and table data
const form = ref({
  name: "",
  email: "",
  gender: "",
  country: "",
});

// State for table rows (submitted form data)
const tableData = ref([]);

// Function to handle form submission
const submitForm = () => {
  // Push form data to table
  if (
    !form.value.name ||
    !form.value.email ||
    !form.value.gender ||
    !form.value.country
  ) {
    toast.error("Please fill all required fields!");
    return;
  }

  // Validate email format
  const emailRegex = /^[\w-]+(\.[\w-]+)*@([\w-]+\.)+[a-zA-Z]{2,7}$/;
  if (!emailRegex.test(form.value.email)) {
    toast.error("Invalid email format!");
    return;
  }

  // Validate gender selection
  if (form.value.gender !== "male" && form.value.gender !== "female") {
    toast.error("Please select a gender!");
    return;
  }

  // Validate country selection
  if (form.value.country === "") {
    toast.error("Please select a country!");
    return;
  }

  // Add form data to table and save to
  tableData.value.push({ ...form.value });
  saveToLocalStorage();

  // Show success toast
  toast.success("Form submitted successfully!");

  // Clear form inputs
  form.value.name = "";
  form.value.email = "";

  // Close the modal
  toggleModal();
};

const loadFromLocalStorage = () => {
  const storedData = localStorage.getItem("userData");
  if (storedData) {
    tableData.value = JSON.parse(storedData);
  }
};
const deleteCredential = (index) => {
  tableData.value.splice(index, 1);
  saveToLocalStorage();
  toast.success("User is deleted successfully!");
};

const saveToLocalStorage = () => {
  localStorage.setItem("userData", JSON.stringify(tableData.value));
};

// Load table data from local storage on component mount
onMounted(() => {
  loadFromLocalStorage();
});
</script>

<template>
  <!-- Button to open the modal -->
  <div class="flex justify-end p-4">
    <button
      @click="toggleModal"
      class="bg-green-500 text-white px-4 py-2 rounded-md hover:bg-green-600">
      Add Info
    </button>
  </div>

  <!-- Table to display form data -->
  <div class="p-4">
    <table class="min-w-full bg-white shadow-md rounded-lg overflow-hidden">
      <thead>
        <tr class="bg-green-500 text-white text-left">
          <th class="py-2 px-4">Name</th>
          <th class="py-2 px-4">Email</th>
          <th class="py-2 px-4">Gender</th>
          <th class="py-2 px-4">Country</th>
          <th class="py-2 px-4">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(data, index) in tableData" :key="index" class="border-b">
          <td class="py-2 px-4">{{ data.name }}</td>
          <td class="py-2 px-4">{{ data.email }}</td>
          <td class="py-2 px-4">{{ data.gender }}</td>
          <td class="py-2 px-4 uppercase">{{ data.country }}</td>
          <td class="py-2 px-4">
            <button
              type="button"
              class="bg-red-500 rounded-lg p-3 text-white"
              @click="deleteCredential(index)">
              Delete
            </button>
          </td>
        </tr>
        <tr v-if="tableData.length === 0">
          <td colspan="2" class="py-4 text-center text-gray-500">
            No data available
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- Modal for adding new info -->
  <teleport to="#teleport-target">
    <transition
      enter-active-class="transition ease-out duration-300"
      enter-from-class="opacity-0 scale-95"
      enter-to-class="opacity-100 scale-100"
      leave-active-class="transition ease-in duration-200"
      leave-from-class="opacity-100 scale-100"
      leave-to-class="opacity-0 scale-95">
      <Modal v-if="showModal" @close="toggleModal">
        <h2 class="text-lg font-bold mb-4">Add New Info</h2>
        <form
          @submit.prevent="submitForm"
          class="max-w-lg mx-auto bg-white p-6 rounded-lg shadow-md space-y-6">
          <!-- Name Field -->
          <div>
            <label for="name" class="block text-gray-700 font-semibold mb-2"
              >Name:</label
            >
            <input
              v-model="form.name"
              type="text"
              id="name"
              name="name"
              class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-green-500" />
          </div>

          <!-- Email Field -->
          <div>
            <label for="email" class="block text-gray-700 font-semibold mb-2"
              >Email:</label
            >
            <input
              v-model="form.email"
              type="text"
              id="email"
              name="email"
              class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-green-500" />
          </div>

          <!-- Gender Field -->
          <div>
            <label for="gender" class="block text-gray-700 font-semibold mb-2"
              >Gender:</label
            >
            <select
              v-model="form.gender"
              type="select"
              id="gender"
              name="gender"
              class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-green-500">
              <option value="">Select Gender</option>
              <option value="male">Male</option>
              <option value="female">Female</option>
              <option value="other">Other</option>
            </select>
          </div>

          <!-- Country Field -->
          <div>
            <label for="country" class="block text-gray-700 font-semibold mb-2"
              >Country:</label
            >
            <select
              v-model="form.country"
              type="select"
              id="country"
              name="country"
              class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-green-500">
              <option value="">Select Country</option>
              <option value="tanzania">Tanzania</option>
              <option value="kenya">Kenya</option>
              <option value="uganda">Uganda</option>
              <option value="rwanda">Rwanda</option>
              <option value="burundi">Burundi</option>
              <option value="malawi">Malawi</option>
              <option value="mozambique">Mozambique</option>
              <option value="zambia">Zambia</option>
              <option value="zimbabwe">Zimbabwe</option>
              <option value="ethiopia">Ethiopia</option>
              <option value="maldives">Maldives</option>
              <option value="swaziland">Swaziland</option>
              <option value="namibia">Namibia</option>
              <option value="botswana">Botswana</option>
              <option value="lesotho">Lesotho</option>
              <option value="sweden">Sweden</option>
              <option value="finland">Finland</option>
              <option value="norway">Norway</option>
              <option value="denmark">Denmark</option>
              <option value="germany">Germany</option>
              <option value="france">France</option>
              <option value="uk">UK</option>
              <option value="us">US</option>
              <option value="australia">Australia</option>
              <option value="new zealand">New Zealand</option>
              <option value="canada">Canada</option>
            </select>
          </div>
          <!-- Submit Button -->
          <div>
            <button
              type="submit"
              class="w-full bg-green-500 text-white font-semibold py-2 rounded-md hover:bg-green-600 transition duration-300">
              Submit
            </button>
          </div>
        </form>
      </Modal>
    </transition>
  </teleport>
</template>

<style scoped>
/* You can add any additional styles if needed */
</style>
