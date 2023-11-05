<template>
    <div style="max-height:80vh;">
        <li v-for="(index, message) in messages" :key="index">{{message.role}} : {{message.text}}</li>
        <v-text-field v-model="newMessage"></v-text-field>
        <v-btn @click="sendMessage"></v-btn>
    </div>
</template>

<script>
//import ChatGenerator from "../ai/ChatGenerator.js"

export default {
    name: 'Chat',
    components:{
    },
    data: () => ({
        messages: [],
        newMessage: ""
    }),
    methods:{

        sendMessage(){
            this.messages.push(
                {
                    role: "user",
                    text: this.newMessage
                }
            );

            this.newMessage = ""
        }
    }
}
</script>