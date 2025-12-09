# TUTORIAL 5: TABLE PAGINATION

## STEP 1
- Create **app/components/Pagination.vue**
- Copy and paste the following code
```
<script setup>
import {
    defineProps,
    defineEmits
} from 'vue';

const props = defineProps({
    currentPage: {
        type: Number,
        required: true,
    },
    totalPages: {
        type: Number,
        required: true,
    },
});

const emit = defineEmits(['page-changed']);

const changePage = (page) => {
    if (page > 0 && page <= props.totalPages) {
        emit('page-changed', page);
    }
};
</script>

<template>
<nav aria-label="Page navigation">
    <ul class="pagination">
        <li class="page-item" :class="{ disabled: currentPage === 1 }">
            <a class="page-link" href="#" @click.prevent="changePage(currentPage - 1)">Previous</a>
        </li>
        <li v-for="page in totalPages" :key="page" class="page-item" :class="{ active: currentPage === page }">
            <a class="page-link" href="#" @click.prevent="changePage(page)">{{ page }}</a>
        </li>
        <li class="page-item" :class="{ disabled: currentPage === totalPages }">
            <a class="page-link" href="#" @click.prevent="changePage(currentPage + 1)">Next</a>
        </li>
    </ul>
</nav>
</template>
```

## STEP 2
- Open **app/pages/pengguna/index.vue**
- Copy the following code and paste below `<table class="table">...</table>`
```
<Pagination v-if="totalPages > 1" :current-page="currentPage" :total-pages="totalPages" @page-changed="handlePageChange" />
```

## STEP 3
- Copy and paste the following code to `<script setup lang="ts">...</script>`
```
const currentPage = ref(1)
const itemsPerPage = 3
```

## STEP 4
- Copy the following code and paste below `onMounted()`
```
// Compute total pages
const totalPages = computed(() => {
    return Math.ceil(users.value.length / itemsPerPage)
})

// Slice users for current page
const paginatedUsers = computed(() => {
    const start = (currentPage.value - 1) * itemsPerPage
    const end = start + itemsPerPage
    return users.value.slice(start, end)
})

// Change page
const handlePageChange = (page: number) => {
    if (page >= 1 && page <= totalPages.value) {
        currentPage.value = page
    }
}
```

## STEP 5
- Copy the following code and replace `<tr v-for="(item, index) in users" :key="item.id">`
```
<tr v-for="(item, index) in paginatedUsers" :key="item.id">
```

## STEP 6
- Copy the following code and replace `<th scope="row">{{ index + 1 }}</th>`
```
<th scope="row">{{ (currentPage - 1) * itemsPerPage + index + 1 }}</th>
```

## STEP 7
- Save and run 
```
npm run dev
```

#
[<< Prev](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/-/blob/main/Tutorial%204:%20Edit%20Data.md)
|
[Next >>](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/-/blob/main/Tutorial%206:%20Table%20Filtering.md)
