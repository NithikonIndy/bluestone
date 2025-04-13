<script setup>
import GuestLayout from '@/Layouts/GuestLayout.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';
import { computed } from 'vue';

const form = useForm({
    name: '',
    email: '',
    password: '',
    password_confirmation: '',
});

// เช็คว่า password กับ confirmation ตรงกันไหม
const passwordMismatch = computed(() => {
    return form.password !== form.password_confirmation && form.password_confirmation !== '';
});

const submit = () => {
    if (!passwordMismatch.value) {
        form.post(route('register'), {
            onFinish: () => form.reset('password', 'password_confirmation'),
        });
    }
};
</script>

<template>
    <GuestLayout>

        <Head title="Register" />

        <form @submit.prevent="submit">
            <!-- Name -->
            <div>
                <InputLabel for="name" value="Name" />
                <TextInput id="name" type="text" class="mt-1 block w-full text-gray-400" v-model="form.name" required
                    autofocus autocomplete="name" placeholder="name" />
                <InputError class="mt-2" :message="form.errors.name" />
            </div>

            <!-- Email -->
            <div class="mt-4">
                <InputLabel for="email" value="Email" />
                <TextInput id="email" type="email" class="mt-1 block w-full text-gray-400" v-model="form.email" required
                    autocomplete="username" placeholder="email" />
                <InputError class="mt-2" :message="form.errors.email" />
            </div>

            <!-- Password -->
            <div class="mt-4">
                <InputLabel for="password" value="Password" />
                <TextInput id="password" type="password" class="mt-1 block w-full" :class="{
                    'border-red-500 ring-1 ring-red-300': passwordMismatch,
                    'text-gray-400 border-gray-300': !passwordMismatch
                }" v-model="form.password" required autocomplete="new-password" placeholder="password" />
                <InputError class="mt-2" :message="form.errors.password" />
            </div>

            <!-- Confirm Password -->
            <div class="mt-4">
                <InputLabel for="password_confirmation" value="Confirm Password" />
                <TextInput id="password_confirmation" type="password" class="mt-1 block w-full" :class="{
                    'border-red-500 ring-1 ring-red-300': passwordMismatch,
                    'text-gray-400 border-gray-300': !passwordMismatch
                }" v-model="form.password_confirmation" required autocomplete="new-password"
                    placeholder="confirm password" />
                <InputError v-if="passwordMismatch" class="mt-2 text-red-600" message="Passwords do not match" />

                <div v-else-if="form.password_confirmation && !passwordMismatch" class="text-sm text-green-600 mt-2">
                    Passwords match
                </div>
            </div>

            <!-- Already Registered -->
            <div class="flex items-center justify-end mt-4">
                <Link :href="route('login')"
                    class="underline text-sm text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-gray-100 rounded-md focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 dark:focus:ring-offset-gray-800">
                Already registered?
                </Link>

                
                <PrimaryButton class="ms-4" :class="{ 'opacity-25': form.processing || passwordMismatch }"
                    :disabled="form.processing || passwordMismatch">
                    Register
                </PrimaryButton>
            </div>
        </form>
    </GuestLayout>
</template>
