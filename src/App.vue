<!-- <template>
  <div class="section" data-theme="valentine">
    <div class="flex h-screen">
      <div class="m-auto">
<div class="mockup-window border border-base-300">
  
<div class="text-center">
   <div class="avatar">
          <div class="w-24 rounded-full ring ring-primary ring-offset-base-100 ring-offset-2">
              <img src="/image/tink.jpg" class="rounded">
          </div>
   </div>  
</div>
        <b><h5 class="text-3xl text-center">YAOWAPA BOONCHOONGAM (FILM)</h5></b>

     <div tabindex="0" class="collapse collapse-arrow border border-base-300 bg-base-200">
  <div class="collapse-title text-xl font-medium text-center">
      ABOUT ME            
         
</div>
  <div class="collapse-content text-center"> 
            I am a fourth year undergraduate student. I am seeking an
            opportunity in the field<br />
            of IT in order to gain technical experience regarding my field of
            <br />
            my study and I am ready to learn and devote myself to the work I am
            giving.
     </div>
</div>
        <div class="stats bg text-primary-content">
          <div class="stat" >
            <b><h5 class="text-1xl">EDUCATION</h5></b>
            <p>
              Mahanakorn university of technology<br />Bachelor of Infoemation
              Science 
              Technology and Digital Innovation (2020-Present)
              <ul>
                <li>&nbsp; &nbsp; Cumulative: 3.19</li>
                <li>&nbsp; &nbsp; Coursewoke: Computer <br>&nbsp; &nbsp; 
                Programming, Web Programming<br>&nbsp; &nbsp; 
                Data Structures and Algorithms,<br>&nbsp; &nbsp; 
                Database Management, Data Science, <br>&nbsp; &nbsp; 
                Introduction to Computer<br>&nbsp; &nbsp; 
                Network Systems and Statics</li>
              </ul>
            </p>
            <br />
            <b><h5 class="text-1xl">CONTACT</h5></b>
            <p>Tel: 098-123-1234</p>
            <p>Email: Yaowapaff2001@gmail.com</p>
            <p>Address: 12/345 Lampakchi, Nongchok, BKK 10530</p>
          </div>

          <div class="stat">
            <b><h5 class="text-1xl">HARD SKILL</h5></b>
            <p>MySQL, Oracle</p>
            <progress
              class="progress progress-secondary w-56"
              value="90"
              max="100"
            ></progress>
            <p>HTML, CSS, Bootstrap</p>
            <progress
              class="progress progress-secondary w-56"
              value="70"
              max="100"
            ></progress>
            <p>C#, PHP, Java Script</p>
            <progress
              class="progress progress-secondary w-56"
              value="40"
              max="100"
            ></progress>
            <p>Laravel Framework</p>
            <progress
              class="progress progress-secondary w-56"
              value="40"
              max="100"
            ></progress>
            <p>Microsoft Office</p>
            <progress
              class="progress progress-secondary w-56"
              value="100"
              max="100"
            ></progress>
            <br />
            <b><h5 class="text-1xl">SOFT SKILL</h5></b>
            <ul>
            <li>Teamwork</li>
            <li>Adaptability</li>
            <li>Creativity</li>
            <li>Work ethic</li>
            <p>Problem-solving</p>
            <p>Leadership</p>
          
            </ul>
          </div>
        </div>

        <footer class="footer footer-center p-4 bg-base-300 text-base-content">
          <div>
            <p>THANK YOU</p>
          </div>
        </footer>
      </div>
    </div>
  </div>
  </div>
 

</template> -->

<!-- 
  <script setup>
</script>

<style scoped>
.section {
  height: 100vh;
}
</style> -->

<!-- ///////////////////////////////////////////////////////// LAB3 ///////////////////////////////////////////////////////// -->

<!-- <template>
<table class="table">
  <thead>
    <tr>
      <td>ID</td>
      <td>Product name</td>
      <td>Product price</td>
    </tr>
  </thead>
  <tbody>
    <tr class="hover" v-for="(product, index) in products" :key="index">
  <td>{{index + 1}}</td>
  <td>{{ product.name }}</td>
  <td>{{ product.price }}</td>
</tr>
  </tbody>
</table>

</template>

<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";
const products = ref()
onMounted(async () => {
  products.value = await getAllProduct();
  
});

const getAllProduct = async () => {
  let data = [];
  data = await axios.get("http://localhost:5000/getallproduct").then((response) => {
    return response.data;
  });
  return data;
}
</script> -->

<!-- ///////////////////////////////////////////////////////// LAB4 ///////////////////////////////////////////////////////// -->


<template>
  <header>
    <div class="navbar bg-base-100">
      <div class="navbar-start">
        <div class="dropdown">
          <label tabindex="0" class="btn btn-ghost lg:hidden">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h8m-8 6h16" /></svg>
          </label>
          <ul tabindex="0" class="menu menu-sm dropdown-content mt-3 z-[1] p-2 shadow bg-base-100 rounded-box w-52">
            <li v-for="menu in menuList" :key="menu.name"><RouterLink :to="menu.path" >{{ menu.name }}</RouterLink></li>
          </ul>
        </div>
        <a class="btn btn-ghost normal-case text-xl">daisyUI</a>
      </div>
      <div class="navbar-center hidden lg:flex">
        <ul class="menu menu-horizontal px-1">
          <li v-for="menu in menuList" :key="menu.name"><RouterLink :to="menu.path" >{{ menu.name }}</RouterLink></li>
        </ul>
      </div>
      <!-- <div class="navbar-end">
        <a class="btn">Button</a>
      </div> -->
    </div>
  </header>
  <div class="container mx-auto">
    <RouterView :key = "router.currentRoute.value.name"/>
  </div>
</template>

<script setup>
import { RouterView, RouterLink, useRouter } from 'vue-router';
const router = useRouter()
const menuList = [
  { path:"/", name: "Home"},
  { path:"/create", name: "Create"},
  { path:"/chat", name: "Chat"}
]

</script>

<style scoped>

</style>




