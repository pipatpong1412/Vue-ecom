<script setup>
import UserLayout from '@/layouts/UserLayout.vue'
import { useCartStore } from '@/stores/user/carts';
import { reactive } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

const cartStore = useCartStore()

const formData = [
    {
        name: 'Email address',
        field: 'email'
    },
    {
        name: 'Name',
        field: 'name'
    },
    {
        name: 'Address',
        field: 'address'
    },
    {
        name: 'Note',
        field: 'note'
    }
]

const userFormData = reactive({
    email: '',
    name: '',
    address: '',
    note: ''
})

const router = useRouter()

const payment = () => {
    cartStore.placeorder(userFormData)
    router.push({ name: 'success' })

}
</script>

<template>
    <div>
        <UserLayout>
            <h1 class="text-3xl font-bold m-4">Shopping Cart</h1>
            <div class="flex">
                <section class="flex-auto w-64 bg-base-200 p-4">
                    <div v-for="form in formData" class="form-control w-full ">
                        <div class="label">
                            <span class="label-text">{{ form.name }}</span>
                        </div>
                        <div v-if="form.field === 'address'">
                            <textarea v-model="userFormData[form.field]" placeholder="Type here"
                                class="textarea textarea-bordered w-full rounded-none"></textarea>
                        </div>
                        <div v-else>
                            <input v-model="userFormData[form.field]" type="text" placeholder="Type here"
                                class="input input-bordered w-full rounded-none" />
                        </div>
                    </div>
                    <button @click="payment()" class="btn btn-neutral w-full mt-4">ชำระเงิน</button>
                </section>
                <section class="flex-auto w-32 bg-slate-200">
                    Summary
                    <div v-for="item in cartStore.items" class="flex bg-white m-">
                        <div class="flex-1">
                            <img class="w-full p-8" :src="item.imageUrl">
                        </div>
                        <div class="flex-1">
                            <div class="flex flex-col justify-between h-full">
                                <div><b>{{ item.name }}</b></div>
                                <div>จำนวน: {{ item.quantity }}</div>
                                <RouterLink :to="{ name: 'cart' }">Edit</RouterLink>
                            </div>
                        </div>
                    </div>
                    <div class="divider"></div>
                    <div class="p-4">
                        <div><b>Order Summary</b></div>
                        <div class="flex justify-between">
                            <div>ราคาสินค้าทั้งหมด</div>
                            <div>{{ cartStore.summaryPrice }}</div>
                        </div>
                        <div class="flex justify-between">
                            <div>ค่าส่ง</div>
                            <div>0</div>
                        </div>
                        <div class="divider"></div>
                        <div>
                            <div class="flex justify-between">
                                <div>ราคาทั้งหมด</div>
                                <div>{{ cartStore.summaryPrice }}</div>
                            </div>
                        </div>
                    </div>
                </section>
            </div>
        </UserLayout>
    </div>
</template>