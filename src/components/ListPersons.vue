<script setup>
const person = defineProps(["name", "url"])

</script>

<template>
    <div class="col-4">
        <div class="card mb-1">
            <img src="" class="card-img-top" alt="Card image cap">
            <div class="card-body">
                <h5 class="card-title">{{ person.name }}</h5>
                <a :href="url" class="btn btn-primary">Detalhes</a>
            </div>
        </div>
    </div>
</template>

<style>

</style>