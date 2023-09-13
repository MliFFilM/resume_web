<script setup>
import { onMounted, ref } from 'vue';
import {
    app,
    auth,
    database,
    ref as refDb,
    set,
    push
} from '../firebaseConfig'

let chat = ref("");
const studentId = "6314110020"
const onSend = () => {
    push(refDb(database, 'all_chat/group_/1'),{
        "user":studentId,
        "message": chat.value,
        "dateTime": new Date().toISOString()
    });
    chat.value ="";
    
}

// onMounted(() => {
//     push(refDb(database,'test'), {
//     "6314110027": "Hello World"
//   });
// });
</script>
<template>
    
    <div class="flex">
        <div class="overflow-y-scroll bg-white h-[90vh] w-[30%]">
            <div class="w-full bg-[#E0DAE3] h-36 border-cyan-700 border-4" v-for="i in 100" :kay="i">
            </div>
        </div>
        <div class="overflow-gray-50 h-[90vh] w-[70%]">
            <div class="overflow-y-scroll h-[95%]">
                <div class="chat chat-start">
                    <div class="chat-bubble">It's over Anakin, <br />I have the high ground.</div>
                </div>
                <div class="chat chat-end">
                    <div class="chat-bubble">You underestimate my power!</div>
                </div>
            </div>
            <div class="flex h-[10%]">
                <input v-on:keyup.enter="onSend" v-model="chat" type="text" placeholder="Enter chat" class="input input-bordered w-[80%] h-[50%]" />
                <button @click="onSend" class="w-[20%] btn h-[50%]">send</button>
            </div>
        </div>
    </div>
</template>
<style scoped></style>
