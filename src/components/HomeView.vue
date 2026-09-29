<script setup>
import codeThinkingImage from "../assets/undraw_code-thinking_tqs9.svg";

const emit = defineEmits(["navigate"]);

const topics = [
    {
        title: "Web Development",
        description: "Learn the basics of HTML, CSS and JavaScript and start creating responsive websites."
    },
    {
        title: "Programming",
        description: "Improve your programming knowledge with languages such as Python and C++."
    },
    {
        title: "Cloud & DevOps",
        description: "Explore technologies such as Docker, Kubernetes and Git used in modern software development."
    }
];

const learningGuide = [
    ["HTML", "Web Development", "Beginner", "Structuring webpages"],
    ["CSS", "Web Development", "Beginner", "Styling webpages"],
    ["JavaScript", "Programming", "Intermediate", "Interactive websites"],
    ["Python", "Programming", "Beginner", "General programming"],
    ["Docker", "Cloud & DevOps", "Intermediate", "Application containers"],
    ["Kubernetes", "Cloud & DevOps", "Advanced", "Container management"]
];
</script>

<template>
    <div>
        <section class="hero">
            <div class="container">
                <h2>Learn. Build. Grow.</h2>
                <p>
                    DevHub is a place for developers to discover useful resources,
                    learn new technologies and improve their programming skills.
                </p>

                <div class="hero-buttons">
                    <button class="button" @click="emit('navigate', 'resources')">
                        Explore Resources
                    </button>
                    <button class="button" @click="emit('navigate', 'community')">
                        Join Community
                    </button>
                </div>
            </div>
        </section>

        <section class="about">
            <div class="container">
                <h2>About DevHub</h2>
                <div class="about-content">
                    <p>
                        DevHub is designed for students, beginner developers and
                        experienced professionals who want a simple place to explore
                        programming and development topics. From building websites to
                        learning programming languages and cloud technologies, DevHub
                        helps developers find useful learning resources in one place.
                    </p>
                    <img :src="codeThinkingImage" alt="Illustration of a developer thinking through code">
                </div>
            </div>
        </section>

        <section class="categories">
            <div class="container">
                <h2>Explore Topics</h2>

                <div class="card-grid">
                    <article
                        v-for="topic in topics"
                        :key="topic.title"
                        class="topic-card"
                    >
                        <h3>{{ topic.title }}</h3>
                        <p>{{ topic.description }}</p>
                    </article>
                </div>
            </div>
        </section>

        <section class="learning-guide">
            <div class="container">
                <h2>Learning Guide</h2>
                <p class="section-description">
                    Use this guide to compare some of the technologies available
                    on DevHub and decide where to start learning.
                </p>

                <div class="table-container">
                    <table>
                        <thead>
                            <tr>
                                <th>Technology</th>
                                <th>Category</th>
                                <th>Difficulty</th>
                                <th>Main Use</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="item in learningGuide" :key="item[0]">
                                <td>{{ item[0] }}</td>
                                <td>{{ item[1] }}</td>
                                <td>{{ item[2] }}</td>
                                <td>{{ item[3] }}</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </section>
    </div>
</template>
