<template>
    <div>
        <div class="container mx-auto">
            <div class="py-8">
                <div>
                    <h2 class="text-2xl font-semibold leading-tight">Profile List</h2>
                    <div class="mt-2">
                        {{ userIds }}
                    </div>
                </div>
                <div class="py-4 overflow-x-auto">
                    <div class="inline-block min-w-full shadow rounded-lg overflow-hidden">
                        <table class="min-w-full leading-normal">
                            <thead>
                                <tr>
                                    <th class="px-3 py-3 border-b-2 border-gray-200 bg-gray-100 font-semibold text-gray-600 uppercase tracking-wider">
                                        <label class="inline-flex items-center">
                                            <span class="pe-1">SL</span>
                                            <input v-model="allSelectedUser" type="checkbox" class="form-checkbox w-4 h-4 text-green-600" @click="selectAllUser">
                                        </label>
                                    </th>
                                    <th class="px-3 py-3 border-b-2 border-gray-200 bg-gray-100 text-xs font-semibold text-gray-600 uppercase tracking-wider">
                                        Name
                                    </th>
                                    <th class="px-3 py-3 border-b-2 border-gray-200 bg-gray-100 text-xs font-semibold text-gray-600 uppercase tracking-wider">
                                        Email
                                    </th>
                                    <th class="px-3 py-3 border-b-2 border-gray-200 bg-gray-100 text-xs font-semibold text-gray-600 uppercase tracking-wider">
                                        Status
                                    </th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(user, index) in users" :key="user.id">
                                    <td class="px-3 py-2 border-b border-gray-200 bg-white text-sm">
                                        <label class="inline-flex items-center">
                                            <span class="pe-1">{{ index + 1 }}.</span>
                                            <input v-model="userIds" type="checkbox" class="form-checkbox w-4 h-4 text-green-600" :value="user.id" @click="selectUser">
                                        </label>
                                    </td>
                                    <td class="px-3 py-2 border-b border-gray-200 bg-white text-sm">
                                        <NuxtLink :to="`/profile/${user.username}`" class="flex items-center no-underline hover:underline">
                                            <div class="flex-shrink-0 w-10 h-10 hidden sm:table-cell">
                                                <img class="w-full h-full rounded-full" :src="`https://ui-avatars.com/api/?background=random&name=${user.name}`" alt="" />
                                            </div>
                                            <div class="ml-3 d-flex items-center">
                                                <p class="text-gray-900 whitespace-no-wrap m-0">
                                                    {{ user.name }}
                                                </p>
                                            </div>
                                        </NuxtLink>
                                    </td>
                                    <td class="px-3 py-2 border-b border-gray-200 bg-white text-sm">
                                        <p class="text-gray-900 whitespace-no-wrap">
                                            {{ user.email }}
                                        </p>
                                    </td>
                                    <td class="px-3 py-2 border-b border-gray-200 bg-white text-sm">
                                        <span class="bg-purple-200 text-purple-600 py-1 px-3 rounded-full text-xs">Active</span>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                allSelectedUser: false,
                userIds: []
            }
        },
        computed:{
            users(){
                return this.$store.getters.getUsers
            }
        },
        methods: {
            selectAllUser() {
                this.userIds = [];
                if (this.allSelectedUser) {
                    this.allSelectedUser = false;
                }
                else{
                    this.users.forEach(user => {
                        this.userIds.push(user.id.toString());
                    })
                }
            },
            selectUser() {
                this.allSelectedUser = false;
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>