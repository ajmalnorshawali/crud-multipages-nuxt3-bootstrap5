# TUTORIAL 8: FORM VALIDATION

## STEP 1
- Open terminal and run the following code
```
npm install @vuelidate/core @vuelidate/validators
```

## STEP 2
- Open **app/pages/pengguna/tambah-pengguna.vue**
- Copy and paste the following code into `<script setup lang="ts">...</script>`
```
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
import Swal from 'sweetalert2'

// Import Vuelidate Core + Validators
import useVuelidate from '@vuelidate/core'
import { required, email } from '@vuelidate/validators'
```

- Copy and paste the following code into `<script setup lang="ts">...</script>`
```
// VUELIDATE RULES
const rules = computed(() => ({
    peopleName: {
        required
    },
    peopleEmail: {
        required,
        email
    },
}))
const v$ = useVuelidate(rules, createForm)
```

## STEP 3
- Copy the following code and replace `const save = async () => {...}`
```
const save = async () => {
    v$.value.$touch()
    if (v$.value.$invalid) {
        console.log('Form invalid!')
        return
    }

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
        await Swal.fire({
            position: 'center',
            icon: 'success',
            text: 'Data Berjaya Disimpan.',
            showConfirmButton: false,
            timer: 3000,
        });

        // Clear form
        createForm.value.peopleName = ''
        createForm.value.peopleEmail = ''

        // Reset validation states
        v$.value.$reset()

        // Navigate to /pengguna
        router.push('/pengguna')
    } catch (error) {
        console.error('Error:', error)
        await Swal.fire({
            position: 'center',
            icon: 'error',
            text: 'Data Gagal Disimpan.',
            showConfirmButton: false,
            timer: 3000,
        });
    }
}
```

## STEP 3
- Copy the following code and replace `<form>...</form>`
```
<form>
    <div class="mb-3">
        <label class="form-label">Nama Pengguna</label>
        <input v-model="createForm.peopleName" class="form-control" :class="{ 'is-invalid': v$.peopleName.$error }">
        <div v-if="v$.peopleName.$error" class="invalid-feedback">
            Nama Pengguna diperlukan
        </div>
    </div>

    <div class="mb-3">
        <label class="form-label">Emel Pengguna</label>
        <input v-model="createForm.peopleEmail" class="form-control" :class="{ 'is-invalid': v$.peopleEmail.$error }">
        <div v-if="v$.peopleEmail.$error" class="invalid-feedback">
            <div v-if="v$.peopleEmail.$errors.some(e => e.$validator === 'required')">
                Emel Pengguna diperlukan
            </div>
            <div v-else-if="v$.peopleEmail.$errors.some(e => e.$validator === 'email')">
                Emel tidak sah
            </div>
        </div>
    </div>

    <button type="button" class="btn btn-primary" @click="save">Simpan</button>
</form>
```

## STEP 4
- Save and run 
```
npm run dev
```

#
[<< Prev](https://code.cloud-connect.asia/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/-/blob/main/Tutorial%207:%20Confirmation%20Message.md)
