<script setup>
import communityImage from "../assets/devs-working-together.jpg";

const emit = defineEmits(["navigate"]);

const features = [
    {
        title: "Ask Questions",
        description: "Get help with programming problems and discuss different approaches with other developers."
    },
    {
        title: "Share Projects",
        description: "Show the projects you are working on and get feedback from other members of the community."
    },
    {
        title: "Learn Together",
        description: "Discover useful tools, resources and ideas shared by developers learning similar technologies."
    }
];
</script>

<template>
    <div>
        <section class="page-intro community-hero">
            <div class="container">
                <h2>Connect. Share. Build Together.</h2>
                <p>
                    DevHub Community gives students, beginner developers and
                    experienced professionals a place to share ideas, ask
                    questions and learn from each other.
                </p>
            </div>
        </section>

        <section class="community-features">
            <div class="container">
                <h2>Be Part of the Community</h2>

                <div class="card-grid">
                    <article
                        v-for="feature in features"
                        :key="feature.title"
                        class="community-card"
                    >
                        <h3>{{ feature.title }}</h3>
                        <p>{{ feature.description }}</p>
                    </article>
                </div>
            </div>
        </section>

        <section class="community-info">
            <div class="container">
                <h2>Why Join DevHub?</h2>

                <div class="community-info-content">
                    <div>
                        <p>
                            Learning development can sometimes be difficult when
                            working alone. DevHub makes it easier to connect with
                            other learners and exchange useful knowledge.
                        </p>
                        <p>
                            Members can discuss web development, programming and
                            cloud technologies while sharing their progress and
                            professional experiences.
                        </p>
                        <button
                            class="primary-button"
                            @click="emit('navigate', 'contribute')"
                        >
                            Contribute to DevHub
                        </button>
                    </div>

                    <img
                        :src="communityImage"
                        alt="Developers working together"
                    >
                </div>
            </div>
        </section>
    </div>
</template>
