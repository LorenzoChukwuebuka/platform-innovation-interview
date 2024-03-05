 <script setup>
 import navComponent from '@/components/navComponent.vue';
</script>

<template>
    <main>
        <navComponent/>
        <div class="card mx-auto mt-5 container w-50">
            <div class="card-body mx-auto">
                <h5 class="card-title">Quiz App</h5>
                <p class="card-text">Welcome to The Quiz App</p>
                <router-link to="/quiz" class="btn btn-sm btn-primary">Click here to start</router-link>
            </div>
        </div>
    </main>
</template>
