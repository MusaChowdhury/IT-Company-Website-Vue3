<template>
  <nav class=" bg-white dark:bg-gray-500">
    <div class="flex flex-wrap justify-between items-center mx-auto w-full px-4 md:px-6 py-2.5">
      <div class="flex items-center  p-2 dark:bg-gray-200 dark:rounded-lg">
        <img src="/logo.png" class="h-14"  />
      </div>
      <div class="flex items-center px-4">
        <div class="flex flex-col items-center justify-center gap-2">
        <a :href= "'tel:'+phone_number" class="mr-6 w-full text-sm font-medium text-blue-900 dark:text-white hover:underline">{{ "Call: "+ phone_number }}</a>
        <p class="mr-6 text-sm w-full font-medium text-blue-900 dark:text-white hover:underline"> {{'Email: '+ email }}</p>
      </div>
        <div class="flex flex-wrap justify-between items-center mx-auto max-w-screen-xl px-4 md:px-6 py-2.5">
          <button type="button" @click="dark">
            <svg v-if="dark_mode.state" class="w-6 h-6" fill="none" stroke="white" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z">
              </path>
            </svg>
            <svg v-if="!dark_mode.state" class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"></path>
            </svg>
          </button>
        </div>
      </div>
    </div>
  </nav>
  <nav class="bg-blue-900 dark:bg-gray-800">
    <div class="max-w-screen-xl px-4 py-2 mx-auto md:px-6">
      <div class="flex items-center">
        <ul class="flex flex-row mt-0 mr-6 space-x-8 text-sm font-medium">
          <li v-for="i in Object.keys(menu_items)">
            <button v-if="! (menu_items[i].constructor == Object)" class="text-white dark:text-white hover:underline"
              @click="hook_of_other_element(menu_items[i],i)">{{ i }}</button>
            <div v-else>
              <button @click="show_hide(i)" class="flex items-start text-white dark:text-white hover:underline">{{
                i
              }}
                <svg class="w-5 h-5 ml-1" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20">
                  <path fill-rule="evenodd"
                    d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                    clip-rule="evenodd"></path>
                </svg></button>

              <div hidden :ref="(el) => { drop_box_store[i] = { el: el, is_shown: 0 } }"
                class="z-[999999] font-normal bg-white divide-y border-blue-200 border-2 divide-gray-100 rounded shadow w-44 dark:bg-gray-700 dark:divide-gray-600 absolute mt-3">
                <ul class="py-1 text-sm text-gray-700 dark:text-gray-400">
                  <li v-for="x in  Object.keys(menu_items[i])">
                    <button @click="hook_of_other_element(menu_items[i][x],x)"
                      class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600  w-full dark:hover:text-white">{{x}}</button>
                  </li>
                </ul>
              </div>





            </div>
          </li>
        </ul>
      </div>

    </div>


  </nav>





</template>





<script setup>
import { reactive, ref } from 'vue'

let dark_mode = reactive({ 'state': false })




const props = defineProps(['menu_items', 'company_name', 'phone_number', 'email'])

let menu_items = props.menu_items
let company_name = props.company_name
let phone_number = props.phone_number
let email = props.email

let drop_box_store = {

}

function hook_of_other_element(function_,value) {
  make_other_box_close("all")
  function_(String(value).toLowerCase())

}


function show_hide(name) {
  if (drop_box_store[name]["is_shown"] == 0) {
    drop_box_store[name]["is_shown"] = 1
  }
  else drop_box_store[name]["is_shown"] = 0

  if (drop_box_store[name]["is_shown"] == 0) {
    
    drop_box_store[name]["el"].classList.remove("bounce-enter")
    setTimeout(() => { drop_box_store[name]["el"].classList.add("bounce-leave") }, 100)
    setTimeout(() => { drop_box_store[name]["el"].style.display = 'none' }, 470)

  }
  else {
    drop_box_store[name]["el"].style.display = 'block'
    drop_box_store[name]["el"].classList.add("bounce-enter")
    drop_box_store[name]["el"].classList.remove("bounce-leave")

  }
  make_other_box_close(name)
  // console.log(drop_box_store[name]["el"])

}

function make_other_box_close(current) {

  for (let i of Object.keys(drop_box_store)) {
    if (i !== current) {
      drop_box_store[i]["is_shown"] = 0
      drop_box_store[i]["el"].style.display = 'none'

      // drop_box_store[i]["el"].classList.remove("bounce-enter")
      // setTimeout( () =>  {drop_box_store[i]["el"].classList.add("bounce-leave")},70)
      // setTimeout( () =>  {drop_box_store[i]["el"].style.display = 'none'},370)
    }
    else if (i === "all") {
      // drop_box_store[i]["el"].classList.remove("bounce-enter")
      // setTimeout( () =>  {drop_box_store[i]["el"].classList.add("bounce-leave")},70)
      // setTimeout( () =>  {drop_box_store[i]["el"].style.display = 'none'},370)
    }

  }
}





function dark() {


  // if set via local storage previously
  if (localStorage.getItem('color-theme')) {
    if (localStorage.getItem('color-theme') === 'light') {
      document.documentElement.classList.add('dark');
      localStorage.setItem('color-theme', 'dark');
    } else {
      document.documentElement.classList.remove('dark');
      localStorage.setItem('color-theme', 'light');
    }

    // if NOT set via local storage previously
  } else {
    if (document.documentElement.classList.contains('dark')) {
      document.documentElement.classList.remove('dark');
      localStorage.setItem('color-theme', 'light');
    } else {
      document.documentElement.classList.add('dark');
      localStorage.setItem('color-theme', 'dark');
    }
  }
  dark_mode.state = document.documentElement.classList.contains('dark')
  
}

</script>



<style scoped>
.bounce-enter {
  animation: bounce-in 0.5s;
}

.bounce-leave {
  animation: bounce-in 0.6s reverse;

}

@keyframes bounce-in {
  0% {
    transform: scale(0);
  }

  50% {
    transform: scale(1.25);
  }

  100% {
    transform: scale(1);
  }
}
</style>
