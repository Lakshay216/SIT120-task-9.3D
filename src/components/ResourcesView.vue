<script setup>
const resources = [
    {
        title: "Web Development",
        description: "Start building websites by learning the core technologies used on the web.",
        tools: ["HTML", "CSS", "JavaScript"]
    },
    {
        title: "Programming",
        description: "Develop problem-solving skills and learn how to create programs using popular programming languages.",
        tools: ["Python", "C++", "JavaScript"]
    },
    {
        title: "Cloud & DevOps",
        description: "Learn about tools used to manage, deploy and maintain modern software applications.",
        tools: ["Git", "Docker", "Kubernetes"]
    }
];
</script>

<template>
    <div>
        <section class="page-intro">
            <div class="container">
                <h2>Developer Resources</h2>
                <p>
                    Explore useful technologies and learning areas that can help
                    you build your development skills and start creating your
                    own projects.
                </p>
            </div>
        </section>

        <section class="resources-section">
            <div class="container resource-grid">
                <article
                    v-for="resource in resources"
                    :key="resource.title"
                    class="resource-card"
                >
                    <h3>{{ resource.title }}</h3>
                    <p>{{ resource.description }}</p>
                    <ul>
                        <li v-for="tool in resource.tools" :key="tool">
                            {{ tool }}
                        </li>
                    </ul>
                </article>
            </div>
        </section>
    </div>
</template>
