<template>
    <button ref="dropdownList" type="button" @click="toggleDropdown()" :class="dynamicClass">
        {{ selected ? selected : default_caption }}
        <svg 
            xmlns="http://www.w3.org/2000/svg" 
            width="30" height="30" 
            viewBox="0 0 30 30" 
            id="down-drop-arrow"
            class="inline-block ml-2 -mr-1">
            <path d="M8.71 11.71l2.59 2.59c.39.39 1.02.39 1.41 0l2.59-2.59c.63-.63.18-1.71-.71-1.71H9.41c-.89 0-1.33 1.08-.7 1.71z"></path>
        </svg>
    </button>
    <div v-if="isOpen" class="dropdown-list overflow-auto milkstore04-text text-xs text-black absolute z-10 mt-2 w-48 bg-white border border-gray-300 rounded-md shadow-lg">
        <ul>
            <li v-for="(e, index) in options" :key="index" @click="selectOption(e)" class="py-2 px-4 cursor-pointer hover:bg-gray-100">
                {{ e }}
            </li>
        </ul>
    </div>    
    
</template>

<script>
export default {
    props: ['options','text_type','default_caption','type']
    , 
    components: {
    },
    data() {
        return {
            isOpen: false,
            selected: null,
            options: this.options,
            data:''
        };
    },
    mounted() {
        document.addEventListener('click', this.handleClickOutside);
    },
    beforeDestroy() {
        document.removeEventListener('click', this.handleClickOutside);
    },
    methods: {
        toggleDropdown() {
            this.isOpen = !this.isOpen;
        },
        selectOption(option ) {
            this.selected = option;
            this.isOpen = false;
            this.$emit('input', this.selected); // Emit the selected option to the parent component
        },
        updateSelected(event) {
            this.selected = event.target.value;
            this.$emit('input', this.selected); // Emit the selected value to the parent component
        },
        handleClickOutside(event) {
            // Close dropdown if clicked outside of it
            if (!this.$refs.dropdownList.contains(event.target)) {
                this.isOpen = false;
            }
        }
        
    },
    computed: {
        dynamicClass() {
            return this.text_type ? this.text_type + ' flex flex-row items-center justify-between text-black border border-custom-700 px-4 bg-white h-10 focus:outline-none focus:border-blue-500 w-full' : 'flex flex-row items-center justify-between text-black border border-red-700 px-4 bg-white h-10 focus:outline-none focus:border-blue-500 w-full';
        }
    }
};
</script>