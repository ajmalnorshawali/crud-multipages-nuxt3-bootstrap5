# TUTORIAL 7: CONFIRMATION MESSAGE

## STEP 1
- Open terminal and run the following code
```
npm install sweetalert2
```

## STEP 2
- Open **app/pages/pengguna/tambah-pengguna.vue**
- Copy and paste the following code into `<script setup lang="ts">...</script>`
```
import Swal from 'sweetalert2'
```

- Copy the following code and replace `const save = async () => {...}`
```
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
- Open **app/pages/pengguna/index.vue**
- Copy and paste the following code into `<script setup lang="ts">...</script>`
```
import Swal from 'sweetalert2'
```

- Copy the following code and replace `const remove = async (id: string) => {...}`
```
// DELETE DATA
const remove = async (id: string) => {
    const result = await Swal.fire({
        title: 'Hapus Data?',
        text: 'Adakah anda pasti untuk hapus data ini?',
        icon: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Ya, hapus!',
        cancelButtonText: 'Batal'
    })

    if (result.value) {
        try {
            const response = await fetch(API_URL + `/${id}`, {
                method: 'DELETE'
            })

            if (!response.ok) throw new Error('Failed to save data')

            console.log('Deleted successfully')
            await Swal.fire({
                position: 'center',
                icon: 'success',
                text: 'Berjaya Dihapuskan.',
                showConfirmButton: false,
                timer: 3000,
            });

            // Refresh the list
            getData()
        } catch (error) {
            console.error('Error:', error)
        }
    }
}
```

## STEP 4
- Save and run 
```
npm run dev
```

#
[<< Prev](https://github.com/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/blob/main/Tutorial%206_%20Table%20Filtering.md)
|
[Next >>](https://github.com/ajmalnorshawali/crud-multipages-nuxt3-bootstrap5/blob/main/Tutorial%208_%20Form%20Validation.md)
