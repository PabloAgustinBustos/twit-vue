<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username">@{{user.username}}</h1>

            <div v-if="user.isAdmin" class="user-profile__admin-badge">Admin</div>

            <div class="user-profile__follower-count">
                <strong>Followers: </strong> {{followers}}
            </div>

            <form @submit.prevent="tweet" class="user-profile__create-tweet" :class="{'exceeded' : newTweetCount > 180}">
                <label for="newTweet"><strong>New Tweet</strong> {{newTweetCount}}/180</label>
                <textarea v-model="newTweetContent" id="newTweet" row="4"/>

                <div class="user-profile__create-tweet-type">
                    <label for="newTweetType"><strong>Type: </strong></label>
                    
                    <select v-model="selectedTweetType" name="" id="newTweetType">
                        <option v-for="option in tweetTypes" :key="option.value" :value="option.value">{{option.name}}</option>
                    </select>
                </div>
                
                <button :disabled="!newTweetContent || newTweetCount > 180">Tweet!!</button>
            </form>
        </div>
        
        <div class="user-profile__tweets-wrapper">
            <Tweet 
                v-for="tweet in user.tweets" 
                
                :key="tweet.id" 
                :username="this.user.username" 
                :id="tweet.id" 
                :content="tweet.content"

                @favourite="toggleFav"
            />
        </div>
    </div>
</template>

<script>
    import Tweet from "./Tweet.vue"

    export default {
        name: 'UserProfile',
        
        components: {Tweet},

        data(){
            return{
                user: {
                    id: 1,
                    username: "Wablo",
                    firstName: "Pablo",
                    lastName: "Bustos",
                    email: "pablobustos609@gmail.com",
                    isAdmin: true,
                    
                    tweets: [
                        {
                            id: 1,
                            content: "Hello world i've just made a twitter account :D"
                        },
                        {
                            id: 2,
                            content: "I'm testing this amazing app xd"
                        },
                    ]
                },

                tweetTypes: [
                    {value: "draft", name: "Draft"},
                    {value: "instant", name: "Instant Tweet"},
                ],

                newTweetContent: "",
                selectedTweetType: "instant",
                disabled: false,

                followers: 0,
            }
        },

        watch: {
            newTweetCount(newValue, oldValue){
                if(newValue >= 180){
                    this.newTweetContent = this.newTweetContent
                }
            }
        },

        computed: {
            fullName(){
                const {firstName, lastName} = this.user
            
                return `${firstName} ${lastName}`
            },

            newTweetCount(){
                return this.newTweetContent.length
            }
        },

        methods: {
            followUser(){
                this.followers++
            },

            toggleFav(id){
                
            },

            tweet(){
                if(this.newTweetContent && this.selectedTweetType !== "draft"){
                    this.user.tweets.unshift({
                        id: Date.now(),
                        content: this.newTweetContent
                    })
                }
                
                this.newTweetContent = ""
            }
        },

        mounted(){
            this.followUser()
        }
    }
</script>

<style scoped>
    .user-profile{
        padding: 50px 5%;
        display: grid;
        grid-template-columns: 1fr 3fr;
    }
    .user-profile__user-panel{
        margin-right: 50px;
        padding: 20px;
        background-color: white;
        border-radius: 5px;
        border: 1px solid #DFE3E8;
        display: flex;
        flex-direction: column;
        
    }

    .exceeded{
        box-sizing: border-box;
        border: 2px solid red;
    }

    h1{
        margin: 0;
    }

    .user-profile__admin-badge{
        background: rebeccapurple;
        color: white;
        border-radius: 5px;
        margin-right: auto;
        padding: 0 10px;
        font-weight: bold;
    }

    .user-profile__create-tweet{
        border-top: 1px solid black;
        padding: 20px;
        display: flex;
        flex-direction: column;
    }
</style>