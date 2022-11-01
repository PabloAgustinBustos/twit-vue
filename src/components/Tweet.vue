<template>
    <div class="tweet-item" @click="$emit('favourite', id)">
        <div class="user-profile__tweet">
            <div class="tweet-item__user">
                @{{username}}
            </div>

            <div class="tweet-item__content">
                {{content}}
            </div>
        </div>
    </div>    
</template>

<script>
    export default {
        name: "Tweet",

        props: ["username", "content", "id"],
    }
</script>

<style>
    .tweet-item{
        padding: 20px;
        background-color: white;
        border-radius: 5px;
        border: 1px solid #DFE3E8;
        box-sizing: border-box;
        cursor: pointer;
        transition: all 0.25s ease;
    }

    .tweet-item:hover{
        transform: scale(1.1, 1.1);
    }

    .tweet-item__user{
        font-weight: bold;
    }
</style>