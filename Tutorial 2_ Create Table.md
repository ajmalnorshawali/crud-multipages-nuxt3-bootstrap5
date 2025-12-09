# TUTORIAL 2: CREATE TABLE

## STEP 1
- Create **app/pages/pengguna/index.vue**
- Copy and paste the following code
```
<script setup lang="ts"></script>

<template>
<div class="container">
    <div class="row">
        <div class="col-md-12">
            <div class="card">
                <div class="card-header">
                    <h5 class="mb-0">Senarai Pengguna</h5>
                </div>
                <div class="card-body">
                    <NuxtLink to="/pengguna/tambah-pengguna" class="btn btn-primary">
                        Tambah Pengguna
                    </NuxtLink>

                    <!-- filtering here -->

                    <table class="table table-striped">
                        <thead>
                            <tr>
                                <th>#</th>
                                <th>Name</th>
                                <th>Email</th>
                                <th>Action</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(item, index) in users" :key="item._id">
                                <th scope="row">{{ index + 1 }}</th>
                                <td>{{ item.peopleName }}</td>
                                <td>{{ item.peopleEmail }}</td>
                                <td>
                                    <NuxtLink :to="`/pengguna/kemaskini-pengguna/${item.id}`" class="btn btn-link">Edit</NuxtLink>
                                    <button @click="remove(item.id)" class="btn btn-link">Remove</button>
                                </td>
                            </tr>
                        </tbody>
                    </table>

                    <!-- pagination here -->
                </div>
            </div>
        </div>
    </div>
</div>
</template>
```

## STEP 2 
- Copy the following code and replace the `<script setup lang="ts"></script>`
```
<script setup lang="ts">
import { ref, onMounted, computed } from 'vue'

// DEFINE TYPE INTERFACE
interface FeedItem {
    id: string
    peopleName: string
    peopleEmail: string
}
const API_URL = 'https://leave-application.konti.cloud-connect.asia/people'

// STATE VARIABLES
const users = ref<FeedItem[]>([])

// READ DATA
const getData = async () => {
    try {
        const response = await fetch(API_URL)
        const data = await response.json()
        users.value = data
    } catch (error) {
        console.error('Fetch error:', error)
    }
}

// DELETE DATA
const remove = async (id: number | string) => {
    try {
        const response = await fetch(API_URL+`/${id}`, {
            method: 'DELETE'
        })

        if (!response.ok) throw new Error('Failed to save data')

        console.log('Deleted successfully')

        // Refresh the list
        getData()
    } catch(error) {
        console.error('Error:', error)
    }
}

onMounted(() => {
    getData()
})
</script>
```

## STEP 3
- Save and run 
```
npm run dev
```

#
[<< Prev](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/-/blob/main/Tutorial%201:%20Setup%20Nuxt3%20+%20Bootstrap5.md)
|
[Next >>](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/-/blob/main/Tutorial%203:%20Create%20Data.md)
