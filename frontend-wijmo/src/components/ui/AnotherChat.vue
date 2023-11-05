<template>
    <div style="max-height:80vh;">
        <li v-for="(message, index) in messages" :key="index">{{message.role}} : {{message.text}}
        
            <v-btn v-if="message.role == 'system'" @click="doit(message)">실행</v-btn>
        </li>
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
3. If you are unsure about any of arguments from user input, you have to create an error for user to know which argument should be input again.
4. Exclusively use the commands listed in double quotes e.g. "command name"

COMMANDS:

commands are described with OpenAPI3 as below:


paths:
  '/inventories/{id}/update':
    summary: decrease stock operation on inventories
    put:
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                productId:
                  description: productId of this DecreaseStock
                  type: integer
                qty:
                  description: qty of this DecreaseStock
                  type: integer
            examples:
              Example 0:
                value:
                  productId: 123
                  qty: 10
              Example 1:
                value:
                  productId: 456
                  qty: 50
              Example 2:
                value:
                  productId: 789
                  qty: 150
      parameters:
        - name: id
          description: DecreaseStock name
          schema:
            type: integer
          in: path
          required: true
          examples:
            Example 0:
              value: 123
            Example 1:
              value: 456
            Example 2:
              value: 789
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Inventory'
              examples:
                Example 0:
                  value:
                    stock: 20
                    productId: 123
                Example 1:
                  value:
                    stock: 30
                    productId: 456
                Example 2:
                  value:
                    stock: 30
                    productId: 789
          description: DecreaseStock Operation
      operationId: DecreaseStock
      summary: DecreaseStock
      description: DecreaseStock
  /inventories/search/findInventory:
    summary: findInventory query on inventories
    get:
      parameters:
        - name: name
          description: Name
          schema:
            type: integer
          in: query
          required: false
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Inventory'
              examples:
                '1':
                  value:
                    productId: 1
                    stock: 1
          description: FindInventory Query
      operationId: FindInventory
      summary: FindInventory
      description: FindInventory
  /inventories:
    summary: Global operations on inventories
    get:
      tags:
        - inventories
      responses:
        '200':
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Inventory'
              examples:
                inventories_json:
                  value:
                    - productId: 1
                      stock: 1
          description: Get list of inventories
      operationId: GetInventory
      summary: Get list of inventories
    post:
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Inventory'
            examples:
              '1':
                value:
                  productId: 1
                  stock: 1
        required: true
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Inventory'
              examples:
                '1':
                  value:
                    productId: 1
                    stock: 1
          description: Changed Inventory
      operationId: Post Inventory
      summary: Post Inventory
  '/inventories/{id}':
    summary: Specific operation on inventories
    get:
      parameters:
        - examples:
            '1':
              value: 1
          name: id
          description: Inventory name
          schema:
            type: integer
          in: path
          required: true
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Inventory'
              examples:
                '1':
                  value:
                    productId: 1
                    stock: 1
          description: inventories with specified id
      operationId: GetInventoryById
      summary: Get Inventory by id
      description: Get Inventory by id
    patch:
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Inventory'
            examples:
              '1':
                value:
                  productId: 1
                  stock: 1
        required: true
      parameters:
        - examples:
            '1':
              value: 1
          name: productId
          description: Inventory name
          schema:
            type: integer
          in: path
          required: true
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Inventory'
              examples:
                '1':
                  value:
                    productId: 1
                    stock: 1
          description: Changed Inventory
      operationId: PatchInventory
      summary: Patch existing Inventory
    parameters:
      - name: id
        description: Inventory id
        schema:
          type: integer
        in: path
        required: true
openapi: 3.0.2
info:
  title: Inventory
  version: 0.0.1
  description: API definition of inventory
  contact:
    name: creator's name
    url: 'http://creator.homepage.address'
    email: creator@creator.com
  license:
    name: MIT License
    url: 'https://opensource.org/licenses/MIT'
components:
  schemas:
    Inventory:
      title: Root Type for Inventory
      description: The root of the Inventory type's schema.
      type: object
      properties:
        productId:
          description: productId of this Inventory
          type: integer
        stock:
          description: stock of this Inventory
          type: integer
      example:
        productId: 1
        stock: 1
tags:
  - name: Inventory
    description: Inventory resource



ERRORS:

1. Incomplete Arguments: "INCOMPLETE-ARGS"

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
    },
    "error": {
        "name": "error name",
        "speak": "error message to user"
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
                    text: response.thoughts ? response.thoughts.speak :  response.command.name + "을 다음의 아규먼트로 실행합니다: " + JSON.stringify(response.command.args),
                    command: response.command
                })
            })

        },

        doit(message){
            alert(JSON.stringify(message.command))
        }

    }
}
</script>