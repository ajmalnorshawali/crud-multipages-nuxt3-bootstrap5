# TUTORIAL 3: CREATE DATA

## STEP 1
- Create **app/pages/pengguna/tambah-pengguna.vue**
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
                            <input v-model="createForm.peopleName" class="form-control">
                        </div>
                        <div class="mb-3">
                            <label class="form-label">Emel Pengguna</label>
                            <input v-model="createForm.peopleEmail" class="form-control">
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
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

interface PeopleForm {
    peopleName: string
    peopleEmail: string
}
const createForm = ref<PeopleForm>({ // Use the interface here
    peopleName: '',
    peopleEmail: '',
})

// CREATE DATA
const save = async () => {
    const { peopleName, peopleEmail } = createForm.value
    try {
        const response = await fetch('https://leave-application.konti.cloud-connect.asia/people', {
            method: 'POST',
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
        createForm.value.peopleName = ''
        createForm.value.peopleEmail = ''

        // Navigate to /pengguna
        router.push('/pengguna')
    } catch (error) {
        console.error('Error:', error)
    }
}
</script>
```

## STEP 3
- Save and run 
```
npm run dev
```

#
[<< Prev](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/-/blob/main/Tutorial%202:%20Create%20Table.md)
|
[Next >>](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/-/blob/main/Tutorial%204:%20Edit%20Data.md)
