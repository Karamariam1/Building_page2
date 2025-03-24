<template>
    <div class="sidebar">
        <!-- Menu Button -->
        <div class="menu-item" @mouseenter="openSubMenu('Button')">
            <button>
                <span>Button</span>
                <i class="fas fa-chevron-right"></i>
            </button>
            <div class="sub-menu" v-if="activeMenu === 'Button'" @mouseenter="cancelCloseMenu"
                @mouseleave="scheduleCloseMenu">
                <button v-for="(button, index) in buttonOptions" :key="index" draggable="true"
                    @dragstart="onDragStart($event, button)">
                    {{ button.text }}
                </button>
            </div>

        </div>


        <!-- Menu Card -->
        <div class="menu-item" @mouseenter="openSubMenu('Card')">
            <button>
                <span>Card</span>
                <i class="fas fa-chevron-right"></i>
            </button>
            <div class="sub-menu" v-if="activeMenu === 'Card'" @mouseenter="cancelCloseMenu"
                @mouseleave="scheduleCloseMenu">
                <p v-for="option in cardOptions" :key="option" @click="selectOption(option)" draggable="true"
                    @dragstart="handleDragStart(option)">
                    {{ option }}
                </p>
            </div>
        </div>

        <!-- Menu IMG -->
        <div class="menu-item" @mouseenter="openSubMenu('IMG')">
            <button>
                <span>Image</span>
                <i class="fas fa-chevron-right"></i>
            </button>
            <div class="sub-menu" v-if="activeMenu === 'IMG'" @mouseenter="cancelCloseMenu"
                @mouseleave="scheduleCloseMenu">
                <p v-for="option in IMGOptions" :key="option.text" draggable="true"
                    @dragstart="handleDragStart($event, option)">
                    {{ option.text }}
                </p>
            </div>
        </div>
    </div>
</template>

<script>
import { ref } from "vue"; 

export default {
    name: "MySidebar",
    props: {
        emitDroppedButton: Function, // A function to emit the dropped button data to parent
    },
    setup(props) {
        const activeMenu = ref(null);
        const closeMenuTimeout = ref(null);

        const cardOptions = ['Option A', 'Option B'];
        // Options for each menu
        const buttonOptions = [
            { text: 'Button RED', color: 'red' },
            { text: 'Button Green', color: 'green' },
            { text: 'Button Black', color: 'black' }
        ];

        const IMGOptions = [
            { text: 'Image 01', path: '01.png' },
            { text: 'Image 02', path: '02.png' }
        ];

        // Active menu tracking
        const openSubMenu = (menu) => {
            activeMenu.value = menu;
            cancelCloseMenu();
        };

        const scheduleCloseMenu = () => {
            closeMenuTimeout.value = setTimeout(() => {
                activeMenu.value = null;
            }, 500);
        };

        const cancelCloseMenu = () => {
            if (closeMenuTimeout.value) {
                clearTimeout(closeMenuTimeout.value);
                closeMenuTimeout.value = null;
            }
        };

        const selectOption = (option) => {
            console.log("Option sélectionnée:", option);
            activeMenu.value = null;
        };
        // Gérer le début du drag
        const handleDragStart = (event, option) => {
            if (!event.dataTransfer) return; // Vérifie si dataTransfer existe

            event.dataTransfer.setData("text", option.text || "");
            if (option.color) {
                event.dataTransfer.setData("color", option.color);
            }
            if (option.path) {
                event.dataTransfer.setData("image", option.path);
            }
        };

 

        // Gérer le drop et afficher le bouton correspondant
        const handleDrop = (event) => {
            event.preventDefault();
            const optionText = event.dataTransfer.getData("text");
            //const color = event.dataTransfer.getData("color");

            // Vérification si c'est un bouton
            const selectedButton = buttonOptions.value.find(opt => opt.text === optionText);
            if (selectedButton) {
                props.emitDroppedButton({ type: "button", text: selectedButton.text, color: selectedButton.color });
                return;
            }

            // Vérification si c'est une image
            const selectedImg = IMGOptions.value.find(opt => opt.text === optionText);
            if (selectedImg) {
                props.emitDroppedButton({ type: "image", path: selectedImg.path });
                return;
            }

            console.warn("Aucun élément trouvé pour :", optionText);
        };


        const onDragStart = (event, button) => {
            event.dataTransfer.setData("text", button.text);
            event.dataTransfer.setData("color", button.color); // Ajout de la couleur au drag
        };

        return {
            activeMenu,
            openSubMenu,
            scheduleCloseMenu,
            cancelCloseMenu,
            selectOption,
            buttonOptions,
            cardOptions,
            IMGOptions,
            handleDragStart,
            handleDrop,
            onDragStart
        };
    }
};
</script>

<style scoped>
.sidebar {
    width: 200px;
    height: calc(100vh - 60px);
    background-color: rgba(200, 200, 200, 0.8);
    border-radius: 20px;
    padding: 10px;
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 117px;
    left: 7px;
}

.menu-item {
    margin: 15px 0;
    position: relative;
}

.menu-item button {
    width: 100%;
    padding: 15px;
    background-color: #00000065;
    border: none;
    border-radius: 20px;
    cursor: pointer;
    text-align: left;
    font-size: 16px;
    font-weight: bold;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.menu-item button:hover {
    background-color: #d0d0d0;
}

.menu-item button span {
    flex-grow: 1;
    color: rgb(255, 255, 255);
}

.menu-item button i {
    font-size: 18px;
    color: rgb(255, 255, 255);
}

.sub-menu {
    position: absolute;
    top: 0;
    left: 100%;
    width: 150px;
    background-color: rgba(200, 200, 200, 0.9);
    border-radius: 10px;
    padding: 10px;
    margin-left: 10px;
    opacity: 0;
    transform: translateX(10px);
    transition: opacity 0.3s ease, transform 0.3s ease;
}

.menu-item:hover .sub-menu,
.menu-item .sub-menu:hover {
    opacity: 1;
    transform: translateX(0);
}

.sub-menu p {
    padding: 5px;
    cursor: pointer;
}

.sub-menu p:hover {
    background-color: #b0b0b0;
}

/* Zone de drop */
.drop-area {
    width: 200px;
    height: 100px;
    margin-top: 30px;
    border: 2px dashed #ccc;
    display: flex;
    justify-content: center;
    align-items: center;
}

.button-drop {
    padding: 10px;
    border-radius: 5px;
    color: white;
    text-align: center;
}
</style>