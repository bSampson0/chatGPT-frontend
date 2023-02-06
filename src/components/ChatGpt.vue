<template>
    <div class="pa-10">
        <v-responsive max-width="344">
            <v-textarea 
                type="text" 
                v-model="userInput"
                label="Message"
                clearable
                clear-icon="mdi-close-circle"
            ></v-textarea>
        </v-responsive>
        <v-btn @click="sendChatRequest(userInput)">Send Message to ChatGPT</v-btn>
        <div>
            <v-progress-circular
                v-if="isLoading"
                class="pa-6"
                indeterminate
                color="primary"
            ></v-progress-circular>
            <div class="results pa-12 ma-12" v-if="result.length > 0">
                {{ result  }}
            </div>
        </div>
    </div>
</template>

<script lang="ts">
export default {
    data() {
        return {
            result: '',
            userInput: '',
            url: 'http://localhost:6969/',
            isLoading: false,
        }
    },
    methods: {
        async sendChatRequest(message: string) {
            this.isLoading = true;
            console.log(message)
            fetch(this.url, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({message: message})
            })
            .then(response => response.json())
            .then(data => {
                this.result = data.text;
                console.log(data)
                this.isLoading = false
            })
        }
    }
}


</script>

<style scoped>
.results {
    background: #eee;
}
</style>