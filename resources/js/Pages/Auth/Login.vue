<script setup>
import GuestLayout from '@/Layouts/GuestLayout.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';
import { computed } from 'vue';

defineProps({
    canResetPassword: {
        type: Boolean,
    },
    status: {
        type: String,
    },
});

const form = useForm({
    email: '',
    password: '',
    remember: false,
});

const submit = () => {
    form.post(route('login'), {
            onFinish: () => form.reset('password'),
        });
};
</script>

<template>
    <GuestLayout>

        <Head title="Log in" />

        <div v-if="status" class="mb-4 font-medium text-sm text-green-600">
            {{ status }}
        </div>

        <form @submit.prevent="submit">
            <div>
                <h5 class="text-xl font-semibold text-gray-800 text-center mb-4">Login</h5>
                <InputLabel for="email" value="Email" />

                <TextInput id="email" type="email" class="mt-1 block w-full placeholder-gray-400" v-model="form.email"
                    required autofocus autocomplete="username" placeholder="username" />

                <InputError class="mt-2" :message="form.errors.email" />
            </div>

            <div class="mt-4">
                <InputLabel for="password" value="Password" />

                <TextInput id="password" type="password" class="mt-1 block w-full placeholder-gray-400"
                    v-model="form.password" required autocomplete="current-password" placeholder="password" />

                <InputError class="mt-2" :message="form.errors.password" />
            </div>

            <div class="block mt-4">
                <label class="flex items-center">
                    <input type="checkbox" name="remember" v-model="form.remember" />
                    <span class="ms-2 text-sm text-gray-600 dark:text-gray-400 ">Remember me</span>
                </label>
            </div>

            <div class="block my-4">
                <p class="text-sm text-gray-600"> Don’t have an account? <a href="/register"
                        class="underline text-blue-600 hover:text-blue-900">Register now!</a></p>
            </div>

            <div class="flex items-center justify-end mt-4">
                <Link :href="route('password.request')"
                    class="underline text-sm text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-gray-100 rounded-md focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 dark:focus:ring-offset-gray-800">
                Forgot your password?
                </Link>

                <PrimaryButton class="ms-4" :class="{ 'opacity-25': form.processing}" :disabled="form.processing ">
                    Log in
                </PrimaryButton>
            </div>
        </form>

    </GuestLayout>
</template>
