<template>
    <div class="main-container">
        <!-- Zone de travail -->
        <div class="work-area" @dragover.prevent @drop="handleWorkAreaDrop">
            <h3>Plan de travail</h3>
            <div v-for="(item, index) in workAreaItems" :key="index" class="dropped-item"
                :class="{ selected: item.selected }" @click="selectItem(item)">
                <!-- Si c'est un bouton -->
                <button v-if="item.type === 'button'" :style="{ backgroundColor: item.color }">{{ item.text }}</button>
                <!-- Si c'est une image -->
                <img v-else-if="item.type === 'image'" :src="item.path" alt="dropped img" class="dropped-img" />
            </div>

        </div>

        <MySidebarRIGHT :selectOption="selectedItem" @delete-item="deleteSelectedItem" />

    </div>

    <div class="main-component">
        <!-- Sidebar Component -->

        <!-- Zone où les boutons et images seront déposés -->
        <div class="drop-area" @dragover.prevent @drop="handleButtonDrop">
            <div v-for="(item, index) in droppedItems" :key="index" class="button-drop">
                <button v-if="item.type === 'button'" :style="{ backgroundColor: item.color }">{{ item.text }}</button>
                <img v-else-if="item.type === 'image'" :src="item.path" alt="dropped img" class="dropped-img" />
            </div>
        </div>

    </div>
</template>

<script>
import { ref } from "vue";
import MySidebarRIGHT from "./MySidebarR.vue";

export default {
    name: "MainComponent",
    components: {
        MySidebarRIGHT,
    },
    setup() {
        const droppedItems = ref([]); // Stocker les boutons et images déposés
        const workAreaItems = ref([]); // Stocker les éléments déposés dans la zone de travail
        const selectedItem = ref(null); // Élément sélectionné pour suppression

        // Sélectionner un élément dans le plan de travail
        const selectItem = (item) => {
            if (selectedItem.value === item) {
                selectedItem.value = null;
                item.selected = false;
            } else {
                if (selectedItem.value) {
                    selectedItem.value.selected = false; // Désélectionner l'ancien élément
                }
                selectedItem.value = item;
                item.selected = true;
            }
        };



        // Gère l'ajout d'un élément via la sidebar (bouton ou image)
        const handleDroppedButton = (option) => {
            droppedItems.value.push(option); // Ajouter au tableau
        };


        const handleDrop = (event, targetArray) => {
            event.preventDefault();
            const optionText = event.dataTransfer.getData("text");
            const optionColor = event.dataTransfer.getData("color");
            const imagePath = event.dataTransfer.getData("image");

            if (imagePath) {
                targetArray.value.push({ type: "image", path: imagePath });
            } else if (optionText) {
                targetArray.value.push({ type: "button", text: optionText, color: optionColor || "#2ecc71" });
            }
        };

        const handleWorkAreaDrop = (event) => {
            handleDrop(event, workAreaItems);
        };

        const handleButtonDrop = (event) => {
            handleDrop(event, droppedItems);
        };


        // Supprimer l'élément sélectionné
        const deleteSelectedItem = () => {
            if (selectedItem.value) {
                // Remove the selected item from workAreaItems
                workAreaItems.value = workAreaItems.value.filter(item => item !== selectedItem.value);
                selectedItem.value = null; // Reset the selected item
                console.log('Item deleted');
            } else {
                alert("No item selected to delete.");
            }
        };





        return {
            droppedItems,
            workAreaItems,
            selectedItem,
            selectItem,
            handleDroppedButton,
            handleButtonDrop,
            handleWorkAreaDrop,
            deleteSelectedItem,
            handleDrop
        };
    },
};
</script>

<style scoped>
.selected {
    border: 2px solid red;
    /* Mettre une bordure rouge sur l'élément sélectionné */
}
</style>

<style scoped>
.main-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin-left: 230px;
    margin-right: 230px;
    margin-top: 10px;
    width: calc(100% - 500px);
    height: calc(100vh - 60px);
    background-color: #fff;
    border: 2px solid #ccc;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    border-radius: 10px;
    padding: 20px;
    overflow: hidden;
}

.work-area {
    width: 100%;
    height: 100%;
    transition: transform 0.3s ease;
}

h3 {
    font-size: 24px;
    margin-bottom: 20px;
}

.button-drop {
    padding: 10px;
    border-radius: 5px;
    color: white;
    text-align: center;
}

.drop-area {
    width: 200px;
    height: 150px;
    margin-top: 30px;
    border: 2px dashed #ccc;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    gap: 5px;
}

.dropped-img {
    max-width: 100px;
    max-height: 100px;
    object-fit: contain;
    border-radius: 5px;
}

.selected {
    border: 2px solid red;
    /* Mettre une bordure rouge sur l'élément sélectionné */
}
</style>
