<template>
    <div style="max-height:80vh;">
        <li v-for="(message, index) in messages" :key="index">{{message.role}} : {{message.text}}</li>
        <v-text-field v-model="newMessage"></v-text-field>
        <v-btn @click="sendMessage">send</v-btn>
    </div>
</template>

<script>
import ChatGenerator from "../ai/ChatGenerator.js"

export default {
    name: 'Chat',
    components:{
    },
    data: () => ({
        messages: [],
        newMessage: "고등어 10마리, 새우 10kg 를 각각 5만원, 7만원에 자갈치수산에서 매입했으니까 기록 좀 해줘",
        generator: null,
        input: {}
    }),

    created(){
        this.generator = new ChatGenerator(this, {isStream: true})
        this.generator.previousMessages = [{
            role: 'system',
            content: `
생선가게 ERP 시스템과 사용자간 챗봇을 제공하려고 해.

CONSTRAINTS:

1. ~4000 word limit for short term memory. Your short term memory is short, so immediately save important information to files.
2. If you are unsure how you previously did something or want to recall past events, thinking about similar events will help you remember.
3. No user assistance
4. Exclusively use the commands listed in double quotes e.g. "command name"

COMMANDS:

1. 매입처리: "매입", args: "<거래처ID: string>", "<금액: number>", "<생선종류: string>", madatory args: 거래처, 생선종류
2. 거리처등록: "거래처등록", args: "<거래처ID: string>", "<사업자등록번호: string>"
3. 유저입력이 잘못된 경우 (mandatory 를 유저가 기입하지 않은 경우): "추가정보입력", args: "<입력정보>"


PERFORMANCE EVALUATION:

1. Continuously review and analyze your actions to ensure you are performing to the best of your abilities.
2. Constructively self-criticize your big-picture behavior constantly.
3. Reflect on past decisions and strategies to refine your approach.
4. Every command has a cost, so be smart and efficient. Aim to complete tasks in the least number of steps.

You should only respond in JSON format as described below (don't skip the thoughts and speak at least)

RESPONSE FORMAT:
[{
    "command": {
        "name": "command name",
        "args":{
            "key": "value"
        }
    },
    "thoughts": {
        "text": "thought",
        "reasoning": "reasoning",
        "plan": [short bulleted,list that conveys,long-term plan],
        "criticism": "constructive self-criticism",
        "speak": "thoughts summary to say to user"
    }
}]  
`

        }]
    },
    methods:{

        sendMessage(){
            this.messages.push(
                {
                    role: "user",
                    text: this.newMessage
                }
            );

            this.generator.generate()

            this.newMessage = ""


        },

        onGenerationFinished(responses){

            console.log(responses);

            responses.forEach(response=> {
                this.messages.push({
                    role:'system',
                    text: response.thoughts.speak
                })
            })
        }
    }
}
</script>