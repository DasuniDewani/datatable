<template>
    <div class="pagination">
        <div></div>

        <div class="pagination-action">
            <button @click="prevPage" :disabled="currentPage === 1 || itemsPerPage === 0">&#9664; Previous</button>
            <span>{{ currentPage }}</span>
            <button @click="nextPage" :disabled="currentPage === totalPages || itemsPerPage === 0">Next &#9654;</button>
        </div>
        <div class="rows-per-page">
            <span>Page size : </span>
            <label v-for="option in rowOptions" :key="option">
                <input type="radio" name="rowsPerPage" :value="option" @change="changeRowsPerPage(option)" :checked="itemsPerPage === option">
                {{ option }}
            </label>
            <label>
                <input type="radio" name="rowsPerPage" :value="0" @change="changeRowsPerPage(0)" :checked="itemsPerPage === 0">
                All
            </label>
        </div>
    </div>
</template>

<script>
import { ref, computed, watch } from 'vue';
export default {
    name: 'TablePagination',
    props: {
    totalItems: {
        type: Number,
        default: 0
        }
    },
    emits: ['pageChange', 'itemsPerPageChange'],

    setup(props, { emit }) {
        const currentPage = ref(1);
        const itemsPerPage = ref(10);
        const rowOptions = ref([10, 15, 20]);

        const totalPages = computed(() => {
        console.log(itemsPerPage.value);
        if (itemsPerPage.value === 0) {
            return 1;
        }
            return Math.ceil(props.totalItems / itemsPerPage.value);
        });

        const prevPage = () => {
            if (currentPage.value > 1) {
                currentPage.value -= 1;
                emit('pageChange', currentPage.value);
            }
        };

        const nextPage = () => {
            if (currentPage.value < totalPages.value) {
                currentPage.value += 1;
                emit('pageChange', currentPage.value);
            }
        };

        const changeRowsPerPage = (option) => {
            if(option === 0) {
                itemsPerPage.value = 0;
            }
            emit('itemsPerPageChange', option);
            if (option === 0) {
                currentPage.value = 1;
            }
        };

        watch(itemsPerPage, (newVal) => {
            if (newVal === 0) {
                currentPage.value = 1;
            }
        });

        return {
            currentPage,
            totalPages,
            itemsPerPage,
            rowOptions,
            prevPage,
            nextPage,
            changeRowsPerPage
        }
    }
}
</script>

<style scoped>
.pagination {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    padding: 20px;
    margin-top: 10px;
    align-items: center;
    gap: 80px;
    margin-bottom: 50px;
}

.pagination-action {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 10px;
}

.pagination-action button {
    width: 120px;
    padding: 10px 18px;
    margin: 0 5px;
    color: #000000;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
}

.pagination-action span {
    padding: 8px 16px;
    margin: 0 5px;
    color: #333;
    font-weight: bold;
    border: solid 2px #000000;
    border-radius: 5px;
}

.rows-per-page {
    display: flex;
    justify-content: center;
    margin-left: 5px;
    gap: 10px;
}

.rows-per-page label {
    margin-right: 5px;
}

.rows-per-page input {
    color: #000000;
}

@media (max-width: 768px) {
    .rows-per-page {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 10px;
    }
}
</style>
