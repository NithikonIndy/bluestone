<template>
    <div class="min-h-screen flex flex-col sm:justify-center items-center pt-6 sm:pt-0 bg-cover bg-center"
        style="background-image: url('/storage/bg-office.png');">
        <div>
            <Link href="/">
            <!-- <ApplicationLogo class="w-20 h-20 fill-current text-gray-500" /> -->
            </Link>
        </div>

        <div class="w-full sm:max-w-md mt-6 px-6 py-4 bg-white  sm:rounded-lg">
            <div class="text-center mb-4">
                <a href="https://www.bluestone.co.th/" target="_blank" rel="noopener noreferrer">
                    <img src="/storage/logo2.png" alt="Bluestone Logo" class="mx-auto" />
                </a>
            </div>
            <slot />
            <hr class="my-4 h-0.5 border-t-0 bg-neutral-100 " />
            <p className="text-center text-gray-300 my-5"> contact us</p>
            <div class="grid grid-cols-5 gap-4 my-5">
                <!-- facebook -->
                <div class="p-2 bg-blue-600 text-white rounded-md w-8 h-8 flex items-center justify-center mx-auto">
                    <a href="https://www.facebook.com/Bluestone.co.th/?locale=th_TH" target="_blank"
                        rel="noopener noreferrer">
                        <i class="fab fa-facebook text-xl"></i>
                    </a>
                </div>

                <!-- ig -->
                <div
                    class="p-2 bg-gradient-to-tr from-yellow-400 via-pink-500 to-purple-600 text-white rounded-md w-8 h-8 flex items-center justify-center mx-auto">
                    <a href="https://www.instagram.com/bluestone.co.th/" target="_blank" rel="noopener noreferrer">
                        <i class="fab fa-instagram text-xl"></i>
                    </a>
                </div>

                <!-- line -->
                <div class="p-2 bg-green-600 text-white rounded-md w-8 h-8 flex items-center justify-center mx-auto">
                    <a href="https://line.me/R/ti/p/@coa2199t?oat_content=url" target="_blank"
                        rel="noopener noreferrer">
                        <i class="fab fa-line text-xl"></i>
                    </a>
                </div>

                <!-- Bluestone -->
                <div class="p-2 rounded-md w-8 h-8 flex items-center justify-center mx-auto" style="background-color: #1586c8;">
                    <a href="https://www.bluestone.co.th/" target="_blank" rel="noopener noreferrer">
                        <img src="/storage/logobluestone.png" alt="Bluestone Logo" class="bg-white" />
                    </a>
                </div>

                <!-- youtube -->
                <div class="p-2 bg-red-600 text-white rounded-md w-8 h-8 flex items-center justify-center mx-auto">
                    <a href="https://www.youtube.com/channel/UCQ3mRpetmm5Ek-LLdTjwaNQ" target="_blank"
                        rel="noopener noreferrer">
                        <i class="fab fa-youtube text-xl"></i>
                    </a>
                </div>
            </div>


        </div>
    </div>
</template>

<script setup>
import ApplicationLogo from '@/Components/ApplicationLogo.vue';
import { Link } from '@inertiajs/vue3';

</script>
