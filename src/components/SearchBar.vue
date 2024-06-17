<template>
    <div class="search-container">
        <input v-model="query" @input="updateSearch" placeholder="Search..." />
        <svg class="search-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
            <path d="M10,20C4.477,20,0,15.523,0,10S4.477,0,10,0s10,4.477,10,10c0,2.386-.832,4.571-2.207,6.293l5.707,5.707c.391.391.391,1.023,0,1.414s-1.023.391-1.414,0l-5.707-5.707C14.571,19.168,12.386,20,10,20z M10,18c4.411,0,8-3.589,8-8s-3.589-8-8-8-8,3.589-8,8S5.589,18,10,18z"></path>
        </svg>
    </div>
</template>

<script>
    import { ref, watch } from 'vue';

    export default {
        
        name: "SearchBar",
        props: ['searchQuery'],
        emits: ['update:searchQuery'],

        setup(props, { emit }) {
            const query = ref(props.searchQuery);

            const updateSearch = () => {
                emit('update:searchQuery', query.value);
            };

            watch(() => props.searchQuery, (newValue) => {
                query.value = newValue;
            });

            return {
                query,
                updateSearch
            };
        }
    };
</script>

<style scoped>
.search-container {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color:#ffffff;
    padding: 20px;
    border-radius: 8px;
    max-width: 500px;
    margin: 10px auto;
    margin-top: 75px;
}

.search-container input { 
    width: 100%;
    padding: 10px 20px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

.search-container input:focus {
    border-color: #08305a; 
    box-shadow: 0 0 8px rgba(0, 123, 255, 0.2);
}

.search-icon {
    position: relative;
    left: 15px;
    width: 20px;
    height: 20px;
    fill: #a99c9c; 
    pointer-events: none;
    margin-right: 10px;
}
</style>
