<script setup>
import GuestLayout from '@/Layouts/GuestLayout.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import SecondaryButton from '@/Components/SecondaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { Head, useForm } from '@inertiajs/vue3';
import { computed } from 'vue';


// สำหรับแสดง modal
import { ref } from 'vue';

// defineProps({
//     status: {
//         type: String,
//     },
// });

const props = defineProps({
    email: {
        type: String,
        required: true,
    },
    verify: {
        type: String,
        required: true,
    },
});

const form = useForm({
    verify: props.verify,
    email: props.email,
    password: '',
    password_confirmation: '',
});

const passwordMismatch = computed(() => {
    return form.password !== form.password_confirmation && form.password_confirmation !== '';
});


const submitReset = () => {
    form.post(route('password.reset'), {
        onSuccess: () => {
            setTimeout(() => {
            }, 3000); // Toast จะหายไปใน 3 วิ
            closeModal();

        },
        onFinish: () => form.reset('password', 'password_confirmation'),
    });
};

// ตัวแปรสำหรับควบคุมการแสดง modal
const showModal = ref(false);
const emailExists = ref(false);

// ฟังก์ชันที่ใช้ในการแสดง modal
const openModal = () => {
    showModal.value = true;
};

// ฟังก์ชันที่ใช้ในการปิด modal
const closeModal = () => {
    showModal.value = false;
    emailExists.value= false;
};

const checkEmailExists = async () => {
    try {
        const response = await axios.post(route('check-email'), {
            email: form.email,
        });

        if (response.data.exists) {
            emailExists.value = true;
            return true;
        }
    } catch (error) {
        emailExists.value = false;
        if (error.response && error.response.status === 404 && error.response.data.error === 'usernotfound') {
            form.errors.email = 'email not found';

        } else {
            form.errors.email = 'error please re-check email';

        }

        return false;
    }
};




</script>

<template>
    <GuestLayout>

        <Head title="Forgot Password" />

        <div class="mb-4 text-sm text-gray-600 dark:text-gray-400">
            Forgot your password? No problem. Just let us know your email address and we will email you a password reset
            link that will allow you to choose a new one.
        </div>

        <div v-if="status" class="mb-4 font-medium text-sm text-green-600 dark:text-green-400">
            {{ status }}
        </div>

        <form @submit.prevent="openModal">
            <div>
                <InputLabel for="email" value="Email" />

                <TextInput id="email" type="email" class="mt-1 block w-full" v-model="form.email" required autofocus
                    autocomplete="username" />

                <InputError class="mt-2" :message="form.errors.email" />
            </div>

            <div class="grid grid-cols-2">
                <div class="flex items-center justify-start mt-4">
                    <SecondaryButton :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                        <a href="/">Back</a>
                    </SecondaryButton>
                </div>
                <div class="flex items-center justify-end mt-4">
                    <PrimaryButton :class="{ 'opacity-25': form.processing }" :disabled="form.processing"
                        @click="checkEmailExists">
                        Reset Password
                    </PrimaryButton>

                </div>
            </div>
        </form>

        <!-- Modal -->
        <div v-if="showModal && emailExists" class="fixed inset-0 flex items-center justify-center bg-gray-600 bg-opacity-50 z-50">
            <div class="bg-white p-8 rounded-lg shadow-xl w-1/2 max-w-lg">
                <!-- Modal header -->
                <div
                    class="flex items-center justify-between p-4 md:p-5 border-b rounded-t dark:border-gray-600 border-gray-200">
                    <h3 class="text-xl font-semibold text-gray-900 dark:text-white">
                        Reset password for <span :style="{ color: form.email ? '#1586c8' : '#ef4444' }">
                            {{ form.email || '' }}
                        </span>
                    </h3>
                    <button type="button"
                        class="end-2.5 text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white"
                        data-modal-hide="authentication-modal" @click="closeModal">
                        <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                            viewBox="0 0 14 14">
                            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6" />
                        </svg>
                        <span class="sr-only">Close modal</span>
                    </button>
                </div>
                <form class="space-y-4 my-5" action="#">
                    <div>
                        <label for="verify" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">
                            verify code</label>
                        <input type="text" name="verify" id="verify" v-model="form.verify"
                            class="bg-gray-50 border border-gray-300 text-gray-900 placeholder-gray-400 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white"
                            placeholder="verify code" required />
                    </div>
                    <div>
                        <label for="password"
                            class="block mb-2 text-sm font-medium text-gray-900 placeholder-gray-400 dark:text-white">New
                            password</label>
                        <input type="password" name="password" id="password" placeholder="New password"
                            v-model="form.password"
                            class="bg-gray-50 border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white"
                            :class="{
                                'border-red-500 ring-1 ring-red-300': passwordMismatch,
                                'text-gray-900 placeholder-gray-400': !passwordMismatch
                            }" required />
                    </div>
                    <div>
                        <label for="password"
                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Confirm new
                            password</label>
                        <input type="password" name="password" id="password_confirmation"
                            placeholder="Confirm new password" v-model="form.password_confirmation"
                            class="bg-gray-50 border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white"
                            :class="{
                                'border-red-500 ring-1 ring-red-300': passwordMismatch,
                                'text-gray-900 placeholder-gray-400': !passwordMismatch
                            }" required />
                    </div>


                </form>

                <div class="p-4 md:p-5 border-t border-gray-200 rounded-b dark:border-gray-600 flex justify-between">
                    <SecondaryButton @click="closeModal" class="">
                        Cancel
                    </SecondaryButton>
                    <PrimaryButton @click="submitReset" class="ml-auto"
                        :class="{ 'opacity-25': form.processing || passwordMismatch }"
                        :disabled="form.processing || passwordMismatch">
                        Confirm
                    </PrimaryButton>
                </div>

            </div>

        </div>
    </GuestLayout>
</template>
