# TUTORIAL 6: TABLE FILTERING

## STEP 1
- Open **app/pages/pengguna/index.vue**
- Copy the following code and paste above `<table class="table">...</table>`
```
<div class="row">
    <div class="col-md-6">
        <input v-model="searchQuery" class="form-control" type="text" placeholder="Carian...">
    </div>
</div>
```

## STEP 2
- Copy and paste the following code into `<script setup lang="ts">...</script>`
```
const searchQuery = ref('')
```

## STEP 3
- Copy the following code and replace `const totalPages`
```
const totalPages = computed(() => {
    const filtered = users.value.filter((user) => {
        const name = user.peopleName?.toLowerCase() || ''
        const email = user.peopleEmail?.toLowerCase() || ''
        return name.includes(searchQuery.value.toLowerCase()) ||
               email.includes(searchQuery.value.toLowerCase())
    })
    return Math.ceil(filtered.length / itemsPerPage)
})
```

## STEP 4
- Copy the following code and replace `const paginatedUsers`
```
const paginatedUsers = computed(() => {
    const filtered = users.value.filter((user) => {
        const name = user.peopleName?.toLowerCase() || ''
        const email = user.peopleEmail?.toLowerCase() || ''
        return name.includes(searchQuery.value.toLowerCase()) ||
               email.includes(searchQuery.value.toLowerCase())
    })

    const start = (currentPage.value - 1) * itemsPerPage
    const end = start + itemsPerPage
    return filtered.slice(start, end)
})
```

## STEP 5
- Save and run 
```
npm run dev
```

#
[<< Prev](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/-/blob/main/Tutorial%205:%20Table%20Pagination.md)
|
[Next >>](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/-/blob/main/Tutorial%207:%20Confirmation%20Message.md)
