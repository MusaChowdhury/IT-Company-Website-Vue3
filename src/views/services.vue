<template>
    <div>

        <section class="bg-white dark:bg-transparent">
            <div class="py-8 px-4 mx-auto max-w-max sm:py-6 lg:px-8">
                <!-- <div class="max-w-screen-md mb-4 lg:mb-10">
                    <h2 class="mb-4 text-4xl tracking-tight font-extrabold text-gray-900 dark:text-white">
                        {{ heading.title }}</h2>
                    <p class="text-gray-500 sm:text-xl dark:text-gray-100">{{ heading.description  }}</p>
                </div> -->


                <div class="space-y-4 md:grid md:grid-cols-2 lg:grid-cols-3 md:gap-10 md:space-y-0">

                    <TransitionGroup  appear @before-enter="beforeEnter" @enter="enter">
                        <div v-for="i in Object.keys(data)" :key="i" :id="i">
                            <div
                                class="flex justify-center bg-white  items-center mb-4 w-14 h-14  rounded-lg bg-primary-100 lg:h-16 lg:w-16 dark:bg-primary-900 dark:p-2">
                                <img :src="'/' + i + '.svg'" alt="">
                            </div>
                            <h3 class="mb-2 text-xl font-bold dark:text-white">{{ data[i].title }}</h3>
                            <p class="text-gray-500 dark:text-white">{{ data[i].description}}</p>

                        </div>

                    </TransitionGroup>










                </div>



            </div>
        </section>







    </div>


</template>



<script setup>

import { reactive } from 'vue';
import gsap from 'gsap'

let props = defineProps(['data'])

let heading = reactive({ title: "", description: "" })

heading.title = props.data.heading.title
heading.description = props.data.heading.description

let data = props.data.features

const beforeEnter = (el) => {
    el.style.opacity = 0
    el.style.transform = 'translateY(100vh)'
}
const enter = (el, done) => {
    gsap.to(el, {
        opacity: 1,
        y: 0,
        duration: 1,
        onComplete: done,
        delay: (Object.keys(data).indexOf(el.id) +1) * 0.2 ,
        
    })
    
}


</script>


<style>

</style>