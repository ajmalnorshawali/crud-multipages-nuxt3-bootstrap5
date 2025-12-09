# TUTORIAL 4: EDIT DATA

## STEP 1
- Create **app/pages/pengguna/kemaskini-pengguna/[id].vue**
- Copy and paste the following code
```
<script setup lang="ts"></script>

<template>
<div class="container">
    <div class="row">
        <div class="col-md-12">
            <div class="card">
                <div class="card-header">
                    <h5 class="mb-0">Tambah Pengguna</h5>
                </div>
                <div class="card-body">
                    <form>
                        <div class="mb-3">
                            <label class="form-label">Nama Pengguna</label>
                            <input v-model="editForm.peopleName" class="form-control">
                        </div>
                        <div class="mb-3">
                            <label class="form-label">Emel Pengguna</label>
                            <input v-model="editForm.peopleEmail" class="form-control">
                        </div>
                        <button type="button" class="btn btn-primary" @click="save">Simpan</button>
                    </form>
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
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'

const route = useRoute()
const router = useRouter()

const API_URL = 'https://leave-application.konti.cloud-connect.asia/people'

// DEFINE TYPE INTERFACE
interface PeopleForm {
    peopleName: string
    peopleEmail: string
}
const editForm = ref<PeopleForm>({ // Use the interface here
    peopleName: '',
    peopleEmail: '',
})

// READ DATA BY ID
const getDataById = async () => {
    try {
        const response = await fetch(API_URL+`/${route.params.id}`)
        const data = await response.json()

        editForm.value.peopleName = data.peopleName
        editForm.value.peopleEmail = data.peopleEmail
    } catch (error) {
        console.error('Fetch error:', error)
    }
}

// UPDATE DATA
const update = async () => {
    const { peopleName, peopleEmail } = editForm.value
    try {
        const response = await fetch(API_URL+`/${route.params.id}`, {
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                peopleName,
                peopleEmail
            }),
        })

        if (!response.ok) throw new Error('Failed to save data')

        const result = await response.json()
        console.log('Success:', result)

        // Clear form
        editForm.value.peopleName = ''
        editForm.value.peopleEmail = ''

        // Navigate to /pengguna
        router.push('/pengguna')
    } catch (error) {
        console.error('Error:', error)
    }
}

onMounted(() => {
    getDataById()
})
</script>
```

## STEP 3
- Save and run 
```
npm run dev
```

#
[<< Prev](https://github.com/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/blob/main/Tutorial%203_%20Create%20Data.md)
|
[Next >>](https://github.com/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/blob/main/Tutorial%205_%20Table%20Pagination.md)
