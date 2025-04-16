<script setup>
import GuestLayout from '@/Layouts/GuestLayout.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';
import { ref, computed } from 'vue';

const form = useForm({
    name: '',
    email: '',
    password: '',
    password_confirmation: '',
});

const passwordMismatch = computed(() => {
    return form.password !== form.password_confirmation && form.password_confirmation !== '';
});

const passwordAtLeast8Chars = computed(() => {
    return form.password.length >= 8;
});

// ฟังก์ชันตรวจสอบอีเมล
const checkEmailExists = async () => {
    try {
        // ส่งคำขอไปที่ route 'check-email'
        const response = await form.post(route('check-email'), {
            email: form.email,
        });
        if (response.exists) {
            // ถ้าอีเมลมีในระบบ
            form.errors.email = 'Email is already registered.';
        }
    } catch (error) {
        console.error('Error checking email:', error);
    }
};

const submit = async () => {
    // ตรวจสอบอีเมลก่อน
    await checkEmailExists();

    // ถ้ามีข้อผิดพลาดในฟอร์ม (เช่น อีเมลซ้ำ)
    if (form.errors.email) return;

    // ส่งข้อมูลการลงทะเบียน
    form.post(route('register'), {
        onSuccess: () => {
            // ทำสิ่งที่ต้องการหลังจากลงทะเบียนเสร็จ (เช่น redirect)
        },
    });
};
</script>

<template>
    <GuestLayout>
        <Head title="Register" />

        <form @submit.prevent="submit">
            <!-- Name -->
            <div>
                <InputLabel for="name" value="Name" />
                <TextInput id="name" type="text" class="mt-1 block w-full text-gray-900 placeholder-gray-400"
                    v-model="form.name" required autofocus autocomplete="name" placeholder="name" />
                <InputError class="mt-2" :message="form.errors.name" />
            </div>

            <!-- Email -->
            <div class="mt-4">
                <InputLabel for="email" value="Email" />
                <TextInput id="email" type="email" class="mt-1 block w-full text-gray-900 placeholder-gray-400"
                    v-model="form.email" required autocomplete="username" placeholder="email" />
                <InputError class="mt-2" :message="form.errors.email" />
            </div>

            <!-- Password -->
            <div class="mt-4">
                <InputLabel for="password" value="Password" />
                <TextInput id="password" type="password" class="mt-1 block w-full" :class="{
                    'border-red-500 ring-1 ring-red-300': passwordMismatch,
                    'text-gray-900 placeholder-gray-400': !passwordMismatch
                }" v-model="form.password" required autocomplete="new-password" placeholder="password" />
                <InputError class="mt-2" :message="form.errors.password" />
                <InputError v-if="form.password && form.password.length<8" class="mt-2 text-red-600" message="Passwords at least 8 character" />
            </div>

            <!-- Confirm Password -->
            <div class="mt-4">
                <InputLabel for="password_confirmation" value="Confirm Password" />
                <TextInput id="password_confirmation" type="password" class="mt-1 block w-full" :class="{
                    'border-red-500 ring-1 ring-red-300': passwordMismatch,
                    'text-gray-900 placeholder-gray-400': !passwordMismatch
                }" v-model="form.password_confirmation" required autocomplete="new-password"
                    placeholder="confirm password" />
                <InputError v-if="passwordMismatch" class="mt-2 text-red-600" message="Passwords do not match" />
                <InputError v-else-if="form.password_confirmation && form.password_confirmation.length<8" class="mt-2 text-red-600" message="Passwords at least 8 character" />
                <div v-else-if="form.password_confirmation && !passwordMismatch" class="text-sm text-green-600 mt-2">
                    Passwords match
                </div>
            </div>

            <div class="flex p-4 my-4 text-sm text-blue-800 rounded-lg bg-blue-50 dark:bg-gray-800 dark:text-blue-400"
                role="alert">
                <svg class="shrink-0 inline w-4 h-4 me-3 mt-[2px]" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
                    fill="currentColor" viewBox="0 0 20 20">
                    <path
                        d="M10 .5a9.5 9.5 0 1 0 9.5 9.5A9.51 9.51 0 0 0 10 .5ZM9.5 4a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3ZM12 15H8a1 1 0 0 1 0-2h1v-3H8a1 1 0 0 1 0-2h2a1 1 0 0 1 1 1v4h1a1 1 0 0 1 0 2Z" />
                </svg>
                <span class="sr-only">Info</span>
                <div>
                    <span class="font-medium">Ensure that these requirements are met:</span>
                    <ul class="mt-1.5 list-disc list-inside">
                        <li>At least 8 characters</li>
                    </ul>
                </div>
            </div>

            <!-- Already Registered -->
            <div class="flex items-center justify-end mt-4">
                <Link :href="route('login')"
                    class="underline text-sm text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-gray-100 rounded-md focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 dark:focus:ring-offset-gray-800">
                Already registered?
                </Link>

                <PrimaryButton class="ms-4" :class="{ 'opacity-25': form.processing || passwordMismatch || !passwordAtLeast8Chars }"
                    :disabled="form.processing || passwordMismatch || !passwordAtLeast8Chars">
                    Register
                </PrimaryButton>
            </div>
        </form>
    </GuestLayout>
</template>
