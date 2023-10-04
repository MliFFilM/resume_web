<script setup>
import { onMounted, onUpdated, ref, watch } from 'vue';
import {
    app,
    auth,
    database,
    ref as refDb,
    set,
    push,
    onValue,
    remove,
    child,
    get
} from '../firebaseConfig'
// import { watch } from 'fs';

let chat = ref("")
const db = refDb(database, 'all_chat/')
let histories = ref([]);
let historykey = ref("")
const bottomEl = ref([])
const studentId = "-0-"
const onSend = () => {
    if (chat.value != '' && historykey.value != '') {

        push(refDb(database, `all_chat/${historykey.value}`), {
            "user": studentId,
            "message": chat.value,
            "dateTime": new Date().toISOString()
        });
        chat.value = ""
    }
}

onValue(db, (snapshot) => {
    const data = snapshot.val() ?? [];
    histories.value = data;
})
// watch(histories, (newHistories, oldHistories) => {
//     bottomEl.value.scrollIntoView({ behavior: 'smooth' })
// })
onUpdated(() => {
    bottomEl.value.scrollIntoView({ behavior: 'smooth' })
})
const selectGroup = (key) => {
    historykey.value = key
}
// onMounted(() => {
//     push(refDb(database, 'test'), {
//         "Belly": "Jerramaiar"
//     });
// });
let groupChatName = ref("");
const createGroup = () => {
    if (groupChatName.value != '') {
        get(child(db, `${groupChatName.value}`)).then((snapshot) => {
            if (snapshot.exists()) {
                alert("Cannot creat chat becuase chat is exists.")
            } else {
                push(refDb(database, `all_chat/${groupChatName.value}`), {
                    "user": studentId,
                    "message": '',
                    "dateTime": new Date().toISOString()
                });
                groupChatName.value = '';
            }
        }).catch((err) => {
            console.error(err);
        })

    }
}

const deleteGroup = (groupName) => {
    if (window.confirm("Do you really want to delete?")) {
        if (groupName) {
            const groupRef = refDb(database, `all_chat/${groupName}`)
            remove(groupRef)
        }
    }
}
</script>

<template>
    <div class="flex">
        <div class=" bg-white h-[90vh] w-[30%]">
            <div class="w-full h-[10%]">
                <button class="btn w-full h-full my-2 shadow-lg" data-theme="cupcake" onclick="my_modal_1.showModal()">Add
                    Group</button>
                <dialog id="my_modal_1" class="modal">
                    <div class="modal-box">
                        <div class="form-control w-full">
                            <label class="label">
                                <span class="label-text">Group Name</span>
                            </label>
                            <input v-model="groupChatName" type="text" placeholder="Type Here"
                                class="input input-bordered w-full" />
                        </div>
                        <div class="modal-action">
                            <form method="dialog">
                                <!-- if there is a button in form, it will close the modal -->
                                <button @click="createGroup" class="btn mr-3">Save</button>
                                <button class="btn">Close</button>
                            </form>
                        </div>
                    </div>
                </dialog>
            </div>
            <div class="overflow-y-scroll w-full h-[90%] pt-4">
                <div class="card card-side bg-base-100 shadow-xl w-full mb-4 h-fit " style="cursor: pointer;"
                    v-for="(group, index) in histories" :key="index" data-theme="cupcake" @click="selectGroup(index)">
                    <div class="gap-1 card-body flex">
                        <div class="flex justify-between w-full">
                            <div>
                                <h2 class="card-title">{{ index }}</h2>
                                <p class="text-sm">{{ group[Object.keys(group)[Object.keys(group).length - 1]]?.message }}
                                </p>
                            </div>
                            <button  class="btn btn-circle" v-on:click="deleteGroup(index)">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
                                    stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                        d="M6 18L18 6M6 6l12 12" />
                                </svg>
                               
                            </button>
                            <!-- <div class="btn px-2 h-full item-center flex justify-center w-20">
                                <button v-on:click="deleteGroup(index)">delete</button>
                            </div> -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="bg-gray-50 h-[90vh] w-[70%]">
            <div class="overflow-y-scroll h-[94%]">
                <!-- <p v-for="i in 100" :key="i">test {{ i }}</p> -->

                <div class="">

                    <div v-for="(history, index) in histories[historykey]"
                        :class="`chat ${history.user == studentId ? 'chat-end' : 'chat-start'}`" :key="index">
                        <div class="chat-header">
                            {{ history.user }}
                            <time class="text-xs opacity-50">{{ history.dateTime }}</time>
                        </div>
                        <div class="chat-bubble max-w-[45%] break-words">{{ history.message }} </div>
                        <div class="chat-image avatar">
                            <div class="w-10 rounded-full">
                                <img src="/image/123.jpg" />
                            </div>
                        </div>
                    </div>
                    <div ref="bottomEl"></div>
                </div>
            </div>
            <div class="flex h-[6%] gap-1.5 px-1">
                <input v-on:keyup.enter="onSend" v-model="chat" type="text" class="input input-bordered w-[80%] h-full"
                    placeholder="Enter chat" />
                <button @click="onSend" class="w-[20%] btn h-full">send</button>
            </div>
        </div>

</div></template>
<style scoped></style>