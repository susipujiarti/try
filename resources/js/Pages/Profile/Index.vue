<template>
    <BaseLayout>
        <section class="md:px-12 p-8 md:py-16 mt-20">
            <!-- main -->
            <section class="flex flex-col md:flex-row gap-12 justify-between">
                <!-- tabs -->
                <section class="md:w-3/12">
                    <section class="flex flex-col rounded-lg overflow-hidden bg-gray-50 border border-gray-200">
                        <template v-for="tab in tabs" :key="tab.id">
                            <button @click="active = tab.id"
                                :class="{ 'bg-green-200 border-r-4 border-green-700': active === tab.id }"
                                class="text-left py-4 pl-6 text-gray-700 duration-300 hover:bg-green-50"><i
                                    :class="tab.icon" class="w-5 mr-4"></i>
                                {{ tab.name }}</button>
                        </template>
                        <!-- logout -->
                        <Link class="text-left py-3 pl-6 text-gray-700" href="/logout" method="post" as="button">
                        <i class="fa-solid fa-arrow-right-from-bracket w-5 mr-4"></i>
                        Keluar Akun
                        </Link>
                    </section>
                </section>

                <section class="w-full">
                    <!-- view of tab -->
                    <section class="w-full border border-gray-200 rounded-lg bg-gray-50 p-8">
                        <KeepAlive>
                            <component :is="tabs[active].component"></component>
                        </KeepAlive>
                    </section>
                </section>
            </section>
        </section>
    </BaseLayout>
</template>

<script setup>

import { ref, onMounted } from 'vue'
import { Link, usePage } from '@inertiajs/vue3'
import { useNotification } from '@kyvg/vue3-notification'
import BaseLayout from '@/Layouts/BaseLayout.vue'
import Account from '@/Pages/Profile/Partials/Account.vue'
import Favorite from '@/Pages/Profile/Partials/Favorite.vue'
import Activity from '@/Pages/Profile/Partials/Activity.vue'

const { notify } = useNotification()
const page = usePage()
const active = ref(0)

const tabs = [
    { id: 0, name: 'Akun', icon: 'fa-solid fa-user-gear', component: Account },
    { id: 1, name: 'Item Favorit', icon: 'fa-solid fa-bookmark', component: Favorite },
    { id: 2, name: 'Aktivitas', icon: 'fa-solid fa-user-clock', component: Activity }
]

onMounted(() => {
    notify({
        title: 'Pemberitahuan',
        text: `Halo, selamat datang ${page.props.auth.user.name}`
    })
})

</script>