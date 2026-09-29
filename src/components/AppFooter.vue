<script setup>
const emit = defineEmits(["navigate"]);

const footerLinks = [
    { label: "Home", view: "home" },
    { label: "Resources", view: "resources" },
    { label: "Community", view: "community" },
    { label: "Contribute", view: "contribute" }
];
</script>

<template>
    <footer class="site-footer">
        <div class="container footer-content">
            <p>&copy; 2026 DevHub. Built for developers.</p>
            <address>
                <a href="mailto:twitchlakshay@gmail.com">twitchlakshay@gmail.com</a>
                <a href="tel:+61413433550">0413 433 550</a>
            </address>
            <nav aria-label="Footer navigation">
                <button
                    v-for="link in footerLinks"
                    :key="link.view"
                    type="button"
                    @click="emit('navigate', link.view)"
                >
                    {{ link.label }}
                </button>
            </nav>
        </div>
    </footer>
</template>
