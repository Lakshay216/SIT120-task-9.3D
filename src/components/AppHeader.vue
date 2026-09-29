<script setup>
defineProps({
    activeView: {
        type: String,
        required: true
    }
});

const emit = defineEmits(["navigate"]);

const links = [
    { label: "Home", view: "home" },
    { label: "Resources", view: "resources" },
    { label: "Community", view: "community" },
    { label: "Contribute", view: "contribute" }
];
</script>

<template>
    <header class="site-header">
        <div class="container header-content">
            <h1 class="logo">
                <a href="#home" @click.prevent="emit('navigate', 'home')">DevHub</a>
            </h1>

            <nav aria-label="Main navigation">
                <ul>
                    <li v-for="link in links" :key="link.view">
                        <button
                            type="button"
                            :class="{ active: activeView === link.view }"
                            @click="emit('navigate', link.view)"
                        >
                            {{ link.label }}
                        </button>
                    </li>
                </ul>
            </nav>
        </div>
    </header>
</template>
