<script setup lang="ts">
import { ref } from "vue";
import PanelMenu from "primevue/panelmenu";
import Card from "primevue/card";
import Button from "primevue/button";
import Avatar from "primevue/avatar";
import Badge from "primevue/badge";
import Divider from "primevue/divider";

const breadcrumbs = [{ label: "Dashboard" }];

// Sidebar collapse state
const isCollapsed = ref(false);

// Sample nested menu items (Laravel starter kit style)
const menuItems = ref([
    {
        label: "Dashboard",
        icon: "pi pi-home",
        route: "/dashboard",
    },
    {
        label: "Management",
        icon: "pi pi-briefcase",
        items: [
            {
                label: "Users",
                icon: "pi pi-users",
                items: [
                    { label: "All Users", icon: "pi pi-list", route: "/users" },
                    {
                        label: "Roles & Permissions",
                        icon: "pi pi-shield",
                        route: "/roles",
                    },
                ],
            },
            {
                label: "Content",
                icon: "pi pi-file",
                items: [
                    {
                        label: "Posts",
                        icon: "pi pi-file-edit",
                        route: "/posts",
                    },
                    {
                        label: "Categories",
                        icon: "pi pi-tags",
                        route: "/categories",
                    },
                ],
            },
        ],
    },
    {
        label: "Settings",
        icon: "pi pi-cog",
        items: [
            {
                label: "General",
                icon: "pi pi-sliders-h",
                route: "/settings/general",
            },
            { label: "Mail", icon: "pi pi-envelope", route: "/settings/mail" },
        ],
    },
]);

const toggleSidebar = () => {
    isCollapsed.value = !isCollapsed.value;
};
</script>

<template>
    <AppLayout
        title="Dashboard"
        description="Application dashboard."
        :breadcrumbs
    >
        <div class="flex h-screen bg-surface-50 dark:bg-surface-950">
            <!-- Sidebar -->
            <aside
                class="flex flex-col border-r border-surface-200 dark:border-surface-800 bg-surface-0 dark:bg-surface-900 transition-all duration-300"
                :class="isCollapsed ? 'w-16' : 'w-72'"
            >
                <!-- Logo / Brand -->
                <div
                    class="flex items-center gap-3 p-4 h-16 border-b border-surface-200 dark:border-surface-800"
                >
                    <i class="pi pi-box text-primary text-2xl"></i>
                    <span
                        v-if="!isCollapsed"
                        class="text-xl font-bold text-surface-900 dark:text-surface-0"
                    >
                        AdminPanel
                    </span>
                </div>

                <!-- Navigation (PanelMenu for nested links) -->
                <div class="flex-1 overflow-y-auto py-2">
                    <PanelMenu
                        v-if="!isCollapsed"
                        :model="menuItems"
                        class="w-full border-none"
                    >
                        <template #item="{ item }">
                            <a
                                v-ripple
                                class="flex items-center px-4 py-2 cursor-pointer text-surface-700 dark:text-surface-0 hover:bg-surface-100 dark:hover:bg-surface-800 rounded-md mx-2"
                                :href="item.route || '#'"
                            >
                                <i :class="[item.icon, 'text-primary mr-3']" />
                                <span
                                    :class="{ 'font-semibold': item.items }"
                                    >{{ item.label }}</span
                                >
                                <Badge
                                    v-if="item.badge"
                                    class="ml-auto"
                                    :value="item.badge"
                                />
                            </a>
                        </template>
                    </PanelMenu>

                    <!-- Collapsed icon-only menu (simplified) -->
                    <div v-else class="flex flex-col items-center gap-4 py-4">
                        <Button
                            icon="pi pi-home"
                            text
                            rounded
                            aria-label="Dashboard"
                        />
                        <Button
                            icon="pi pi-briefcase"
                            text
                            rounded
                            aria-label="Management"
                        />
                        <Button
                            icon="pi pi-cog"
                            text
                            rounded
                            aria-label="Settings"
                        />
                    </div>
                </div>

                <!-- Sidebar Footer (User) -->
                <div
                    class="p-4 border-t border-surface-200 dark:border-surface-800"
                >
                    <div
                        class="flex items-center gap-3"
                        :class="{ 'justify-center': isCollapsed }"
                    >
                        <Avatar
                            label="JD"
                            shape="circle"
                            class="bg-primary text-primary-contrast"
                        />
                        <div v-if="!isCollapsed" class="flex flex-col">
                            <span
                                class="text-sm font-semibold text-surface-900 dark:text-surface-0"
                                >John Doe</span
                            >
                            <span class="text-xs text-muted-color"
                                >Administrator</span
                            >
                        </div>
                    </div>
                </div>
            </aside>

            <!-- Main Content Area -->
            <div class="flex-1 flex flex-col min-w-0">
                <!-- Top Header -->
                <header
                    class="h-16 border-b border-surface-200 dark:border-surface-800 bg-surface-0 dark:bg-surface-900 flex items-center justify-between px-6"
                >
                    <div class="flex items-center gap-4">
                        <Button
                            :icon="
                                isCollapsed
                                    ? 'pi pi-angle-right'
                                    : 'pi pi-angle-left'
                            "
                            text
                            rounded
                            severity="secondary"
                            @click="toggleSidebar"
                            aria-label="Toggle Sidebar"
                        />
                        <nav
                            class="hidden md:flex items-center gap-2 text-sm text-muted-color"
                        >
                            <span
                                v-for="(crumb, index) in breadcrumbs"
                                :key="index"
                            >
                                {{ crumb.label }}
                            </span>
                        </nav>
                    </div>
                    <div class="flex items-center gap-3">
                        <Button
                            icon="pi pi-bell"
                            text
                            rounded
                            severity="secondary"
                            aria-label="Notifications"
                        />
                        <Button
                            icon="pi pi-user"
                            text
                            rounded
                            severity="secondary"
                            aria-label="Profile"
                        />
                    </div>
                </header>

                <!-- Page Content -->
                <main class="flex-1 overflow-y-auto p-6">
                    <div class="mb-6">
                        <h1
                            class="text-2xl font-bold text-surface-900 dark:text-surface-0"
                        >
                            Dashboard
                        </h1>
                        <p class="text-muted-color mt-1">
                            Welcome back, John. Here is your application
                            overview.
                        </p>
                    </div>

                    <Card>
                        <template #content>
                            <div class="flex items-center gap-4">
                                <i
                                    class="pi pi-check-circle text-green-500 text-4xl"
                                ></i>
                                <div>
                                    <p
                                        class="text-lg font-medium text-surface-900 dark:text-surface-0 m-0"
                                    >
                                        You are logged in!
                                    </p>
                                    <p
                                        class="text-muted-color text-sm m-0 mt-1"
                                    >
                                        This is the professional admin panel
                                        layout using PrimeVue 4.
                                    </p>
                                </div>
                            </div>
                        </template>
                    </Card>

                    <!-- Example Stat Cards (Laravel starter kit style) -->
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mt-6">
                        <Card>
                            <template #content>
                                <div class="flex justify-between items-start">
                                    <div>
                                        <span
                                            class="text-muted-color text-sm block"
                                            >Total Users</span
                                        >
                                        <span
                                            class="text-2xl font-bold text-surface-900 dark:text-surface-0"
                                            >1,248</span
                                        >
                                    </div>
                                    <i
                                        class="pi pi-users text-primary text-2xl"
                                    ></i>
                                </div>
                            </template>
                        </Card>
                        <Card>
                            <template #content>
                                <div class="flex justify-between items-start">
                                    <div>
                                        <span
                                            class="text-muted-color text-sm block"
                                            >New Posts</span
                                        >
                                        <span
                                            class="text-2xl font-bold text-surface-900 dark:text-surface-0"
                                            >36</span
                                        >
                                    </div>
                                    <i
                                        class="pi pi-file-edit text-primary text-2xl"
                                    ></i>
                                </div>
                            </template>
                        </Card>
                        <Card>
                            <template #content>
                                <div class="flex justify-between items-start">
                                    <div>
                                        <span
                                            class="text-muted-color text-sm block"
                                            >Active Roles</span
                                        >
                                        <span
                                            class="text-2xl font-bold text-surface-900 dark:text-surface-0"
                                            >5</span
                                        >
                                    </div>
                                    <i
                                        class="pi pi-shield text-primary text-2xl"
                                    ></i>
                                </div>
                            </template>
                        </Card>
                    </div>
                </main>
            </div>
        </div>
    </AppLayout>
</template>
