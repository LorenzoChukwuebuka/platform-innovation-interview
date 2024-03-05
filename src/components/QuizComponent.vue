<script setup>
import { ref, computed } from "vue";
import quizData from "../../questions.json";
import { shuffleArray } from "@/utils/utils";

const currentQuestionIndex = ref(0);
const userAnswers = ref({});
const showResult = ref(false);
const shuffledQuestions = ref(shuffleArray(quizData.questions));

const score = computed(() => {
    let score = 0;
    shuffledQuestions.value.forEach((question, index) => {
        if (userAnswers.value[index] === question.correctAnswer) {
            score++;
        }
    });
    return score;
});

//move to the next question
const nextQuestion = () => {
    if (currentQuestionIndex.value < quizData.questions.length - 1) {
        currentQuestionIndex.value++;
    } else {
        showResult.value = true;
    }

    console.log(userAnswers)
};

//go back to the previous question
const prevQuestion = () => {
    if (currentQuestionIndex.value > 0) {
        currentQuestionIndex.value--;
    }
};
</script>


<template>
    <div class="container mt-4">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-body">
                        <h1 class="card-title">{{ quizData.quizTitle }}</h1>
                        <div v-if="!showResult">
                            <div
                                v-for="(question, index) in shuffledQuestions"
                                :key="index"
                                v-show="currentQuestionIndex === index"
                            >
                                <h3 class="mt-4">
                                    Question {{ index + 1 }}:
                                    {{ question.question }}
                                </h3>
                                <div
                                    v-for="(
                                        option, optionIndex
                                    ) in question.options"
                                    :key="optionIndex"
                                    class="form-check"
                                >
                                    <input
                                        class="form-check-input"
                                        type="radio"
                                        :value="option"
                                        v-model="userAnswers[index]"
                                        :id="`question-${index}-option-${optionIndex}`"
                                        :name="`question-${index}`"
                                    />
                                    <label
                                        class="form-check-label"
                                        :for="`question-${index}-option-${optionIndex}`"
                                        >{{ option }}</label
                                    >
                                </div>
                            </div>
                            <div
                                class="mt-4"
                                v-if="
                                    currentQuestionIndex ===
                                    shuffledQuestions.length - 1
                                "
                            >
                                <button
                                    class="btn btn-sm btn-primary"
                                    @click="showResult = true"
                                >
                                    Show Result
                                </button>
                            </div>
                            <div
                                class="mt-4 d-flex justify-content-between"
                                v-else
                            >
                                <button
                                    class="btn btn-sm btn-primary"
                                    :disabled="currentQuestionIndex === 0"
                                    @click="prevQuestion"
                                >
                                    Previous
                                </button>
                                <button
                                    class="btn btn-sm btn-primary"
                                    :disabled="
                                        currentQuestionIndex ===
                                        shuffledQuestions.length - 1
                                    "
                                    @click="nextQuestion"
                                >
                                    Next
                                </button>
                            </div>
                            <div class="progress mt-4">
                                <div
                                    class="progress-bar"
                                    role="progressbar"
                                    :style="{
                                        width: `${
                                            ((currentQuestionIndex + 1) /
                                                shuffledQuestions.length) *
                                            100
                                        }%`,
                                    }"
                                    :aria-valuenow="currentQuestionIndex + 1"
                                    aria-valuemin="1"
                                    :aria-valuemax="shuffledQuestions.length"
                                >
                                    Question {{ currentQuestionIndex + 1 }} of
                                    {{ shuffledQuestions.length }}
                                </div>
                            </div>
                        </div>
                        <div v-else>
                            <h2 class="mt-4">Quiz Results</h2>
                            <div
                                v-for="(question, index) in shuffledQuestions"
                                :key="index"
                                class="mt-4"
                            >
                                <h3>
                                    Question {{ index + 1 }}:
                                    {{ question.question }}
                                </h3>
                                <p>Your Answer: {{ userAnswers[index] }}</p>
                                <p
                                    :class="{
                                        'text-success':
                                            userAnswers[index] ===
                                            question.correctAnswer,
                                        'text-danger':
                                            userAnswers[index] !==
                                            question.correctAnswer,
                                    }"
                                >
                                    Correct Answer: {{ question.correctAnswer }}
                                </p>
                                <p
                                    v-if="
                                        userAnswers[index] !==
                                        question.correctAnswer
                                    "
                                >
                                    {{ question.explanation }}
                                </p>
                            </div>
                            <p class="mt-4">
                                Your Score: {{ score }} /
                                {{ shuffledQuestions.length }}
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.quiz-container {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
}

.correct {
    color: green;
}

.incorrect {
    color: red;
}

.navigation {
    margin-top: 20px;
}

.progress {
    margin-top: 10px;
}
</style>