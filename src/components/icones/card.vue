<template>
<div class="bg-red-800 text-white rounded-xl px-4 pt-3 shadow-card">
    <div class="flex">
        <img class="rounded-xl w-1/3" :src="photo" alt="Image de l'artiste">
        <div class="mx-4">
            <h3 class="text-base">{{ Nom }}</h3>
            <p class="text-sm mt-4 md:text-lg lg:text-xl">{{ Bio }}</p>
        </div>
    </div>
    
</div>
</template>
<script>

export default {
    props : {
        Nom: String,
        Bio: String,
        photo:String
    }
};
</script>