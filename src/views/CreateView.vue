<script setup>
import { onMounted,ref } from "vue";
import axios from "axios";
import { useRouter } from 'vue-router'
let name = ref(''),
    price = ref(0),
    routerName = ref(''),
    _id = ref('');
const router = useRouter()
onMounted(() => {
  routerName.value = router.currentRoute.value.name;
  _id.value = router.currentRoute.value.params?.id;
  console.log("routerName", routerName.value);
  console.log("_id",_id.value);
  if(routerName.value == "edit"){
    initEdit();
  }
});
const API_PATH = import.meta.env.VITE_API_PATH;
const initEdit = async () => {
  const {data} =await axios.get(`${API_PATH}/getproduct/${_id.value}`);
  name.value = data.data.name;
  price.value = data.data.price;
}
const Submit = async () => {
  const req = {
    name: name.value,price: price.value,
  };
  if(routerName.value == "create") {
  await axios.post(`${API_PATH}/create`,
   req
   ).then((response) => {
    // alert(response.status);
    name.value = '';
    price.value = '';
  });
}else {
  await axios.post(
    `${API_PATH}/update/${_id.value}`,
    req
  ).then((response) => {
    router.push({name : 'home', replace: true});

  });
  
}
}

const submit = async () => {
  const req ={
    name:name.value, price:price.value
  };
}

</script>


<template>
    <div class="mockup-window border bg-base-300">
        <center>
            <div class="form-control w-full max-w-xs">
                <label class="label">
                    <span class="label-te">Product Name</span>
                </label>
                <input type="text" placeholder="name" class="input input-bordered w-full max-w-xs" v-model="name" />
            </div>
            <div class="form-control w-full max-w-xs">
                <label class="label">
                    <span class="label-te">Product Price</span>
                </label>
                <input type="text" placeholder="price" class="input input-bordered w-full max-w-xs" v-model="price" />
            </div>
            <br>
            <div class="text-center m-3">
                <button class="btn btn-active btn-secondary" v-on:click="Submit()">Add Product</button>
            </div>
        </center>
    </div>
</template>

<style scoped></style>



