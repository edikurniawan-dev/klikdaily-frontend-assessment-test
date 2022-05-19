<script setup>
import { RouterLink, RouterView } from 'vue-router';
import './index.css';
</script>

<template>
    <header>
        <div class="">
            <nav>
                <RouterLink to="/">Home</RouterLink>
            </nav>
        </div>
    </header>

    <RouterView />
</template>
