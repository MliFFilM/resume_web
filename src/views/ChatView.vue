<script setup>
import { onMounted, onUpdated, ref, watch } from 'vue';
import {
    app,
    auth,
    database,
    ref as refDb,
    set,
    push,
    onValue
} from '../firebaseConfig'
// import { DataSnapshot } from '@firebase/database';

let chat = ref("");
let histories = ref([]);
let historykey = ref(""); // collect select chat
const bottomEl = ref(null);
const studentId = "-0-"
let refChat;
const db = refDb(database, 'all_chat/')
const onSend = () => {
    if (chat.value != '' && historykey.value != '') {
        push(refDb(database, `all_chat/${historykey.value}`), {
            "user": studentId,
            "message": chat.value,
            "dateTime": new Date().toISOString()
        });
        chat.value = "";
    }
}

onValue(db, (snapshot) => {
    const data = snapshot.val();
    histories.value = data;
})
onUpdated(() => {
    bottomEl.value.scrollIntoView({ behavior: 'smooth' });
})
const selectGroup = (key) => {
    historykey.value = key;
}
let groupChatName = ref("");
const createGroup = () => {
    if (groupChatName.value != '') {
        push(refDb(database, `all_chat/${groupChatName.value}`), {
            "user": studentId,
            "message": '',
            "dateTime": new Date().toISOString()
        });
        groupChatName.value = '';
    }
}
const deleteGroup = (key) => {
    if (key !== '') {
        const groupRef = refDb(database, `all_chat/${key}`);
        set(groupRef, null); 
        histories.value = histories.value.filter((group, index) => index !== key);
        if (historykey.value === key) {
            historykey.value = '';
        }
    }
};
</script>
 

<template>
    <div class="flex">
        <div class="bg-[#00BFFF] h-[90vh] w-[30%]">
            <div class="overflow-y-scroll w-full h-[95%]">
                <div class="card card-side bg-dase-100 shadow-xl w-full mb-4" style="cursor: pointer;"
                    v-for="(group, index) in histories" :key="index" data-theme="cupcake" @click="selectGroup(index)">
                    <div class="avatar online p-4">
                        <div class="w-20 rounded-full">
                            <img src="/image/123.jpg" />
                        </div>
                    </div>
                    <div class="card-body">
                        <h2 class="card-title">{{ index }}</h2>
                        <p>{{ group[Object.keys(group)[Object.keys(group).length - 1]].message }}</p>
                    </div>
                    <button class="btn btn-circle btn-outline" @click="deleteGroup(index)">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
                            stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>
                    <!-- <button type="button" class="btn btn-btn btn-error" @click="deleteProduct(product._id)">Delete</button> -->
                </div>
            </div>
            <div class="w-full h-[5%]">
                <button class="btn w-full h-full bg-[#191970]" data-theme="business" onclick="my_modal_1.showModal()">Add
                    Group</button>
                <dialog id="my_modal_1" class="modal">
                    <div class="modal-box">
                        <div class="from-control w-full">
                            <label class="label">
                                <span class="label-text">Group Name</span>
                            </label>
                            <input type="text" placeholder="Type here" class="input input-bordered w-full"
                                v-model="groupChatName" />
                        </div>
                        <div class="modal-action">
                            <button class="btn w-full" @click="createGroup">create Group</button>
                            <form method="dialog">
                                <!-- if there is a button in form, it will close the modal -->
                                <button class="btn">Save</button>
                            </form>
                        </div>
                    </div>
                </dialog>
            </div>
        </div>

        <div class="bg-[#F0F8FF] h-[90vh] w-[70%] ">
            <div class="overflow-y-scroll h-[95%]">
                <div v-for="(history, index) in histories[historykey]"
                    :class="`chat ${history.user == studentId ? 'chat-end' : 'chat-start'}`" :key="index">
                    <div class="chat-image avatar">
                        <div class="w-10 rounded-full">
                            <img src="/image/123.jpg" />
                        </div>
                    </div>
                    <div class="chat-header">
                        {{ history.user }}
                        <time class="test-xs opacity-50"> {{ history.dateTime }}</time>
                    </div>
                    <div class="chat-bubble">{{ history.message }}
                    </div>

                </div>

                <div ref="bottomEl"></div>
            </div>

            <div class="flex h-[5%] gap-4">
                <input v-on:keyup.enter="onSend" v-model="chat" type="text" placeholder="Type here"
                    class="input input-bordered w-[80%] h-full" />
                <button @click="onSend" class="w-[20%] btn h-full bg-[#191970] text-white">send</button>
            </div>
    </div>
</div></template>
<style scoped></style>
