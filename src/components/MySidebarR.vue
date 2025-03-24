<!-- RightSidebar.vue -->
<template>
    <div class="sidebar right">
        <div class="control-panel">
            <h3>Control Panel</h3>

            <!-- Delete Button -->
            <button class="delete-btn" @click="deleteAction">Delete</button>

            <!-- Input for Card Title -->
            <div class="input-container">
                <label for="card-title">Card Title</label>
                <input type="text" id="card-title" v-model="cardTitle" placeholder="Enter card title" />
            </div>

            <!-- Input list with filter search --> <!-- Combobox-style input for adding classes -->
            <div class="input-list">
                <label for="classes">Add new class</label>
                <input type="text" v-model="searchQuery" placeholder="Search classes" @focus="showDropdown = true"
                    @blur="handleBlur" />

                <!-- Dropdown list that appears when typing -->
                <ul v-if="showDropdown" class="dropdown">
                    <li v-for="(item, index) in filteredClasses" :key="index" @click="addClass(item)">
                        {{ item }}
                    </li>
                </ul>

                <!-- Display added classes -->
                <div class="added-classes">
                    <h4>Added Classes:</h4>
                    <ul>
                        <li v-for="(classItem, index) in classList" :key="index">{{ classItem }}</li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { ref, computed } from "vue";

export default {
    name: "MySidebarR",
    props: {
        activeMenu: String,
        openSubMenu: Function,
        scheduleCloseMenu: Function,
        cancelCloseMenu: Function,
        selectOption: Object,
    },
    setup(props, { emit }) {
        // State for card title, class list, and search query
        const cardTitle = ref("");
        const searchQuery = ref("");
        const classList = ref([]);
        const showDropdown = ref(false); // Controls visibility of the dropdown

        // List of available classes for filtering
        const availableClasses = ref([
            "Class A", "Class B", "Class C", "Class D", "Class E", "Class F"
        ]);

        // Filter the available classes based on the search query
        const filteredClasses = computed(() => {
            return availableClasses.value.filter(className =>
                className.toLowerCase().includes(searchQuery.value.toLowerCase())
            );
        });

        // Add class to the list
        const addClass = (className) => {
            if (!classList.value.includes(className)) {
                classList.value.push(className);
                searchQuery.value = ''; // Clear the search query after adding
                showDropdown.value = false; // Close the dropdown after selection
            }
        };

        // Handle input blur (closing dropdown after selection)
        const handleBlur = () => {
            // Delay hiding the dropdown to allow click event to trigger
            setTimeout(() => {
                showDropdown.value = false;
            }, 200);
        };

        // Delete action (can be extended for more functionality)
        // Handle the delete action with confirmation
        const deleteAction = () => {
            const confirmDelete = window.confirm("Are you sure you want to delete this item?");
            if (confirmDelete) {
                console.log('delete-item event emitted');
                emit("delete-item");  // Correctement émis avec emit()
                console.log('delete-item event emitted 0000000000');
            }
        };


        return {
            cardTitle,
            searchQuery,
            classList,
            showDropdown,
            availableClasses,
            filteredClasses,
            addClass,
            deleteAction,
            handleBlur,
        };
    },
};
</script>

<style scoped>
/* Right Sidebar Styles */
.sidebar.right {
    position: fixed;
    top: 117px;
    right: 7px;
    width: 250px;
    height: calc(100vh - 60px);
    background-color: rgba(200, 200, 200, 0.8);
    border-radius: 20px;
    padding: 20px;
    display: flex;
    flex-direction: column;
}

.control-panel {
    display: flex;
    flex-direction: column;
    gap: 20px;
}

h3 {
    font-size: 20px;
    margin-bottom: 10px;
}

.delete-btn {
    padding: 10px;
    background-color: red;
    color: white;
    border: none;
    border-radius: 20px;
    cursor: pointer;
    text-align: center;
}

.delete-btn:hover {
    background-color: darkred;
}

.input-container {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.input-container label {
    font-size: 14px;
}

.input-container input {
    padding: 8px;
    border-radius: 10px;
    border: 1px solid #ccc;
}

.input-list {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.input-list input {
    padding: 8px;
    border-radius: 10px;
    border: 1px solid #ccc;
}

.input-list ul {
    max-height: 150px;
    overflow-y: auto;
    padding: 0;
    list-style: none;
}

.input-list li {
    background-color: #f0f0f0;
    padding: 8px;
    margin: 5px 0;
    border-radius: 5px;
}

.input-list li:hover {
    background-color: #e0e0e0;
}
</style>