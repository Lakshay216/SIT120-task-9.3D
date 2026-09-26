<script setup>
import { ref } from "vue";

const props = defineProps({
    formTitle: { type: String, default: "Contribute to DevHub" },
    initialName: { type: String, default: "" },
    roleOptions: { type: Array, required: true }
});

const emit = defineEmits(["submit-form"]);
const experienceOptions = ["Beginner", "Intermediate", "Advanced"];
const contributionTypes = [
    "Ask a Question",
    "Share a Project",
    "Recommend a Resource"
];
const topicOptions = [
    "Web Development",
    "Programming",
    "Cloud & DevOps",
    "Other"
];

const createForm = () => ({
    name: props.initialName,
    email: "",
    role: "",
    experience: "",
    yearsExperience: null,
    contributionType: "",
    topics: [],
    title: "",
    description: "",
    link: "",
    agreement: false
});

const form = ref(createForm());
const feedback = ref({});
const validity = ref({});
const successMessage = ref("");
let resetTimer;

const namePattern = /^[A-Za-zÀ-ÖØ-öø-ÿ' -]{2,50}$/;
const emailPattern = /^[^\s@]+@[^\s@]+\.[A-Za-z]{2,}$/;
const urlPattern = /^https?:\/\/\S+\.\S+$/i;

/* Saves the message and colour used below each field */
function setFeedback(fieldName, valid, message) {
    validity.value[fieldName] = valid;
    feedback.value[fieldName] = message;
    return valid;
}

/* Checks one field when the user leaves it or changes it */
function validateField(fieldName) {
    const value = form.value[fieldName];

    if (fieldName === "name") {
        const valid = namePattern.test(value.trim());
        return setFeedback(fieldName, valid, valid
            ? "Name looks correct."
            : "Enter a name using 2 to 50 letters.");
    }

    if (fieldName === "email") {
        const valid = emailPattern.test(value.trim());
        return setFeedback(fieldName, valid, valid
            ? "Email address looks correct."
            : "Enter an email such as name@example.com.");
    }

    if (fieldName === "role") {
        return setFeedback(fieldName, value !== "", value
            ? "Role selected."
            : "Please select your current role.");
    }

    if (fieldName === "experience") {
        return setFeedback(fieldName, value !== "", value
            ? "Experience level selected."
            : "Please select your experience level.");
    }

    if (fieldName === "yearsExperience") {
        const valid = typeof value === "number" && value >= 0 && value <= 60;
        return setFeedback(fieldName, valid, valid
            ? "Years of experience looks correct."
            : "Enter a number between 0 and 60.");
    }

    if (fieldName === "contributionType") {
        return setFeedback(fieldName, value !== "", value
            ? "Selection confirmed."
            : "Please select one contribution type.");
    }

    if (fieldName === "topics") {
        return setFeedback(fieldName, value.length > 0, value.length
            ? "Selection confirmed."
            : "Please select at least one topic.");
    }

    if (fieldName === "title") {
        const valid = value.trim().length >= 5 && value.trim().length <= 80;
        return setFeedback(fieldName, valid, valid
            ? "Title looks correct."
            : "The title must contain 5 to 80 characters.");
    }

    if (fieldName === "description") {
        const valid = value.trim().length >= 20 && value.trim().length <= 600;
        return setFeedback(fieldName, valid, valid
            ? "Description length is suitable."
            : "The description must contain 20 to 600 characters.");
    }

    if (fieldName === "link") {
        const valid = value.trim() === "" || urlPattern.test(value.trim());
        let message = "No link added. This field is optional.";

        if (value.trim() !== "") {
            message = valid
                ? "Link format looks correct."
                : "Enter a complete http:// or https:// link.";
        }

        return setFeedback(fieldName, valid, message);
    }

    return setFeedback(fieldName, value === true, value
        ? "Community agreement confirmed."
        : "You must confirm the community agreement.");
}

/* Checks all required fields before sending the data */
function validateForm() {
    const fieldNames = [
        "name", "email", "role", "experience", "yearsExperience",
        "contributionType", "topics", "title", "description", "link",
        "agreement"
    ];

    return fieldNames.map(validateField).every(Boolean);
}

function resetForm() {
    clearTimeout(resetTimer);
    form.value = createForm();
    feedback.value = {};
    validity.value = {};
    successMessage.value = "";
}

function submitForm() {
    if (!validateForm()) return;

    emit("submit-form", { ...form.value });
    successMessage.value = "Your contribution was submitted successfully.";

    /* The child form resets, but the parent summary stays visible */
    resetTimer = setTimeout(resetForm, 2000);
}
</script>

<template>
    <section class="form-page">
        <div class="page-intro">
            <div class="container">
                <h2>{{ formTitle }}</h2>
                <p>
                    Ask a question, share a project or recommend a useful
                    resource to the DevHub community.
                </p>
            </div>
        </div>

        <div class="container form-section">
            <form
                class="community-form"
                novalidate
                @submit.prevent="submitForm"
                @reset.prevent="resetForm"
            >
                <p class="required-note">Fields marked with * are required.</p>

                <div class="form-columns">
                    <fieldset>
                        <legend>1 - About You</legend>

                        <div class="form-group">
                            <label for="name">Name *</label>
                            <input
                                id="name"
                                v-model.trim="form.name"
                                type="text"
                                :class="{
                                    'field-valid': validity.name === true,
                                    'field-invalid': validity.name === false
                                }"
                                :aria-invalid="validity.name === false"
                                aria-describedby="name-feedback"
                                @blur="validateField('name')"
                            >
                            <span
                                v-if="feedback.name"
                                id="name-feedback"
                                class="feedback"
                                :class="validity.name ? 'valid-message' : 'invalid-message'"
                                aria-live="polite"
                            >{{ feedback.name }}</span>
                        </div>

                        <div class="form-group">
                            <label for="email">Email Address *</label>
                            <input
                                id="email"
                                v-model.trim="form.email"
                                type="email"
                                :class="{
                                    'field-valid': validity.email === true,
                                    'field-invalid': validity.email === false
                                }"
                                :aria-invalid="validity.email === false"
                                aria-describedby="email-feedback"
                                @blur="validateField('email')"
                            >
                            <span
                                v-if="feedback.email"
                                id="email-feedback"
                                class="feedback"
                                :class="validity.email ? 'valid-message' : 'invalid-message'"
                                aria-live="polite"
                            >{{ feedback.email }}</span>
                        </div>

                        <div class="form-group">
                            <label for="role">Current Role *</label>
                            <select
                                id="role"
                                v-model="form.role"
                                :class="{
                                    'field-valid': validity.role === true,
                                    'field-invalid': validity.role === false
                                }"
                                :aria-invalid="validity.role === false"
                                aria-describedby="role-feedback"
                                @change="validateField('role')"
                            >
                                <option value="">Select your role</option>
                                <option
                                    v-for="role in roleOptions"
                                    :key="role"
                                    :value="role"
                                >{{ role }}</option>
                            </select>
                            <span
                                v-if="feedback.role"
                                id="role-feedback"
                                class="feedback"
                                :class="validity.role ? 'valid-message' : 'invalid-message'"
                                aria-live="polite"
                            >{{ feedback.role }}</span>
                        </div>

                        <div class="form-group">
                            <label for="experience">Experience Level *</label>
                            <select
                                id="experience"
                                v-model="form.experience"
                                :class="{
                                    'field-valid': validity.experience === true,
                                    'field-invalid': validity.experience === false
                                }"
                                :aria-invalid="validity.experience === false"
                                aria-describedby="experience-feedback"
                                @change="validateField('experience')"
                            >
                                <option value="">Select your level</option>
                                <option
                                    v-for="level in experienceOptions"
                                    :key="level"
                                    :value="level"
                                >{{ level }}</option>
                            </select>
                            <span
                                v-if="feedback.experience"
                                id="experience-feedback"
                                class="feedback"
                                :class="validity.experience ? 'valid-message' : 'invalid-message'"
                                aria-live="polite"
                            >{{ feedback.experience }}</span>
                        </div>

                        <div class="form-group">
                            <label for="years">Years of Experience *</label>
                            <input
                                id="years"
                                v-model.number="form.yearsExperience"
                                type="number"
                                min="0"
                                max="60"
                                :class="{
                                    'field-valid': validity.yearsExperience === true,
                                    'field-invalid': validity.yearsExperience === false
                                }"
                                :aria-invalid="validity.yearsExperience === false"
                                aria-describedby="years-feedback"
                                @blur="validateField('yearsExperience')"
                            >
                            <span
                                v-if="feedback.yearsExperience"
                                id="years-feedback"
                                class="feedback"
                                :class="validity.yearsExperience ? 'valid-message' : 'invalid-message'"
                                aria-live="polite"
                            >{{ feedback.yearsExperience }}</span>
                        </div>
                    </fieldset>

                    <fieldset>
                        <legend>2 - Your Contribution</legend>

                        <div class="form-question">
                            <p>Contribution Type *</p>
                            <div
                                class="option-group choice-container"
                                :class="{
                                    'group-valid': validity.contributionType === true,
                                    'group-invalid': validity.contributionType === false
                                }"
                            >
                                <label v-for="type in contributionTypes" :key="type">
                                    <input
                                        v-model="form.contributionType"
                                        type="radio"
                                        name="contribution-type"
                                        :value="type"
                                        @change="validateField('contributionType')"
                                    >
                                    {{ type }}
                                </label>
                            </div>
                            <span
                                v-if="feedback.contributionType"
                                class="feedback"
                                :class="validity.contributionType ? 'valid-message' : 'invalid-message'"
                                aria-live="polite"
                            >{{ feedback.contributionType }}</span>
                        </div>

                        <div class="form-question">
                            <p>Relevant Topics *</p>
                            <div
                                class="option-group choice-container"
                                :class="{
                                    'group-valid': validity.topics === true,
                                    'group-invalid': validity.topics === false
                                }"
                            >
                                <label v-for="topic in topicOptions" :key="topic">
                                    <input
                                        v-model="form.topics"
                                        type="checkbox"
                                        :value="topic"
                                        @change="validateField('topics')"
                                    >
                                    {{ topic }}
                                </label>
                            </div>
                            <span
                                v-if="feedback.topics"
                                class="feedback"
                                :class="validity.topics ? 'valid-message' : 'invalid-message'"
                                aria-live="polite"
                            >{{ feedback.topics }}</span>
                        </div>

                        <div class="form-group">
                            <label for="title">Contribution Title *</label>
                            <input
                                id="title"
                                v-model.trim="form.title"
                                type="text"
                                :class="{
                                    'field-valid': validity.title === true,
                                    'field-invalid': validity.title === false
                                }"
                                :aria-invalid="validity.title === false"
                                aria-describedby="title-feedback"
                                @blur="validateField('title')"
                            >
                            <span
                                v-if="feedback.title"
                                id="title-feedback"
                                class="feedback"
                                :class="validity.title ? 'valid-message' : 'invalid-message'"
                                aria-live="polite"
                            >{{ feedback.title }}</span>
                        </div>

                        <div class="form-group">
                            <label for="description">Description *</label>
                            <textarea
                                id="description"
                                v-model.trim="form.description"
                                rows="6"
                                :class="{
                                    'field-valid': validity.description === true,
                                    'field-invalid': validity.description === false
                                }"
                                :aria-invalid="validity.description === false"
                                aria-describedby="description-feedback description-count"
                                @blur="validateField('description')"
                            ></textarea>
                            <div class="description-information">
                                <span
                                    v-if="feedback.description"
                                    id="description-feedback"
                                    class="feedback"
                                    :class="validity.description ? 'valid-message' : 'invalid-message'"
                                    aria-live="polite"
                                >{{ feedback.description }}</span>
                                <span
                                    id="description-count"
                                    class="character-count"
                                    :class="{ 'count-warning': form.description.length > 600 }"
                                >{{ form.description.length }} / 600 characters</span>
                            </div>
                        </div>

                        <div class="form-group">
                            <label for="link">Project or Resource Link</label>
                            <input
                                id="link"
                                v-model.trim="form.link"
                                type="url"
                                :class="{
                                    'field-valid': validity.link === true,
                                    'field-invalid': validity.link === false
                                }"
                                :aria-invalid="validity.link === false"
                                aria-describedby="link-help link-feedback"
                                @blur="validateField('link')"
                            >
                            <small id="link-help">
                                Optional. Include a link if you are sharing a
                                project or resource.
                            </small>
                            <span
                                v-if="feedback.link"
                                id="link-feedback"
                                class="feedback"
                                :class="validity.link ? 'valid-message' : 'invalid-message'"
                                aria-live="polite"
                            >{{ feedback.link }}</span>
                        </div>
                    </fieldset>
                </div>

                <div
                    class="agreement"
                    :class="{
                        'group-valid': validity.agreement === true,
                        'group-invalid': validity.agreement === false
                    }"
                >
                    <label>
                        <input
                            v-model="form.agreement"
                            type="checkbox"
                            @change="validateField('agreement')"
                        >
                        I confirm that my contribution is appropriate for the
                        DevHub community. *
                    </label>
                    <span
                        v-if="feedback.agreement"
                        class="feedback"
                        :class="validity.agreement ? 'valid-message' : 'invalid-message'"
                        aria-live="polite"
                    >{{ feedback.agreement }}</span>
                </div>

                <p v-if="successMessage" class="success-message" aria-live="polite">
                    {{ successMessage }}
                </p>

                <div class="form-buttons">
                    <button class="reset-button" type="reset">Reset Form</button>
                    <button class="submit-button" type="submit">Submit</button>
                </div>
            </form>
        </div>
    </section>
</template>
