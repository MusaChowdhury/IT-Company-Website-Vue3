<template>
    <div class="flex h-full w-full flex-col">
    <div v-if="!no_child.state" class="flex flex-row mt-5 w-full justify-center items-center">
        <p class="text-center text-base text-blue-700 dark:text-gray-200">Contact us (email/phone) for further
            information. Prices may vary based on requirements.</p>
    </div>
    <div class="flex flex-col h-full min-w-[100%] justify-center items-start flex-nowrap overflow-auto">



        <div v-if="!no_child.state"
            class="flex p-10 flex-col grow  justify-center  lg:flex-row  gap-16 w-fit items-center lg:w-max lg:h-max">
            <TransitionGroup :css="false" appear @enter="enter" @after-enter="end">
                <div v-for="i in data[target]" :key='i' :id="(data[target]).indexOf(i)"
                    class="flex flex-col h-full w-full p-0 gradient-border flex-nowrap  ">
                    <div
                        class="flex flex-col grow flex-nowrap w-full  h-full text-center text-gray-900 bg-white p-4 shadow border border-gray-100  dark:border-gray-600  dark:bg-gray-800 dark:text-white border_round">
                        <h3 class="mb-4 text-2xl font-semibold">{{ i.title }}</h3>
                        <p class="font-light text-gray-500 sm:text-lg dark:text-gray-400">{{ i.description }}</p>
                        <div class="flex justify-center items-baseline my-8">
                            <span class="mr-2 text-5xl font-extrabold">{{ i.money }}</span>
                        </div>
                        <!-- List -->
                        <ul role="list" class=" space-y-2 text-left">
                            <li class="flex items-center space-x-3" v-for="f in i.features">
                                <!-- Icon -->
                                <svg class="flex-shrink-0 w-5 h-5 text-green-500 dark:text-green-400"
                                    fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                                    <path fill-rule="evenodd"
                                        d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                                        clip-rule="evenodd"></path>
                                </svg>
                                <span>{{ f }}</span>
                            </li>


                        </ul>

                    </div>
                </div>
            </TransitionGroup>

        </div>

        <div v-if="no_child.state"
            class="flex  justify-items-start justify-start  w-full h-full pt-[10%] px-[5%] pb-[10%] min-h-[30%]  bg-cover bg-right-bottom fade-bg"
            style="background-image: url('/contact.webp');">
            <div
                class="flex flex-col  flex-wrap min-w-[45%] max-w-[90%]   bg-black p-4  bg-opacity-75 sm:bg-gray-900 rounded-lg justify-center shrink sm:bg-opacity-50">
                <h1 class="text-left mb-4 text-5xl font-extrabold leading-none tracking-tight text-blue-200 ">
                    Contact Us.
                </h1>
                <p class="text-left mb-6 text-xl font-normal pl-3  text-gray-100 whitespace-pre-line">
                    {{ data[target][0].description }}</p>
            </div>
        </div>





    </div>
</div>



</template>




<script setup>

import gsap from 'gsap'
import { reactive } from 'vue';

let animation_delay
let reverse_animation_delay

let no_child = reactive({ state: false })
let props = defineProps(['data', 'activate',])

let data = props.data



let target = props.activate


if (data[target].length == 1) {
    no_child.state = true

}
else {
    animation_delay = calculate_animation_delay(.3)
    reverse_animation_delay = calculate_reverse_animation_delay(animation_delay)

}









const enter = (el, done) => {

    if (animation_delay[el.id] == "~") // differentiate the middle element
    {
        fade(el, animation_delay[parseInt(el.id) + 1] + 1.5, done)
    }
    else if (animation_delay[el.id] < 0) {
        transition(el, Math.abs(animation_delay[el.id]), "left", done)
    }
    else {
        transition(el, Math.abs(animation_delay[el.id]), "right", done)
    }




}



function end(el) {

    let delay = reverse_animation_delay[el.id] == '~' ? .1 : Math.abs(reverse_animation_delay[el.id]) + .7
    if (data[target].length % 2 == 0) {
        delay -= 1
    }

    gsap.to(el, { "--border-width": ".32em", duration: 0.6, delay: delay });
}


function fade(element, delay, done) {

    gsap.fromTo(element, { opacity: 0 }, { opacity: .999, delay: delay, duration: .5, onComplete: done, lazy: false })
}

function transition(element, delay, direction, done) {
    let direction_t = 1
    if (direction === "left") {
        direction_t = -1
    }
    gsap.fromTo(element, { x: direction_t * window.innerWidth }, { x: 0, delay: delay, duration: 1.5, onComplete: done })
}






function calculate_animation_delay(delay_t) {
    let animation_delay = []
    for (let i = 0; i < Math.floor(data[target].length / 2); ++i) {
        animation_delay.push(((Math.floor(data[target].length) / 2) - i) * -1 * delay_t)
    }

    if (data[target].length % 2 != 0) {
        animation_delay.push("~")
    }
    for (let i = (Math.floor(data[target].length / 2)) - 1; i >= 0; --i) {
        animation_delay.push((animation_delay[i]) * -1)
    }
    return animation_delay
}
function calculate_reverse_animation_delay(animation_delay) {

    let part_1 = []
    let part_2 = []
    let part_1_done = false
    for (let i in animation_delay) {
        if (!part_1_done) {
            if (animation_delay[i] == '~') {

                part_1.reverse()
                part_1.push('~')
                part_1_done = true
            }
            else part_1.push(animation_delay[i])

        }
        else {
            part_2.push(animation_delay[i])
        }


    }
    return part_1.concat(part_2.reverse())
}




</script>




<style scoped >
.gradient-border {
    /* --border-width: 0em; */
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: var(--border-width);
}


.gradient-border::after {
    position: absolute;
    content: "";
    top: calc(-1 * var(--border-width));
    left: calc(-1 * var(--border-width));
    z-index: -1;
    width: calc(100% + var(--border-width) * 2);
    height: calc(100% + var(--border-width) * 2);
    background: linear-gradient(60deg,
            hsl(224, 85%, 66%),
            hsl(269, 85%, 66%),
            hsl(314, 85%, 66%),
            hsl(359, 85%, 66%),
            hsl(296, 32%, 35%),
            hsl(234, 80%, 75%),
            hsl(341, 37%, 55%),
            hsl(179, 85%, 66%));
    background-size: 300% 300%;
    background-position: 0 50%;
    border-radius: calc(2 * var(--border-width));
    animation: moveGradient 1.4s alternate infinite;
}

.border_round {
    border-radius: var(--border-width);
}


@keyframes moveGradient {
    50% {
        background-position: 100% 50%;
    }
}



.fade-bg {
    animation: fadeIn 1s;

}


div.scroll {
    margin: 4px, 4px;
    /* padding: 4px; */
    /* background-color: #08c708; */

    overflow-x: auto;
    overflow-y: hidden;
    white-space: nowrap;
}



@keyframes fadeIn {
    0% {
        scale: 2;
        opacity: 0;

        /* opacity: 0; */
    }

    70% {
        scale: 1;
        opacity: .4;
    }

    100% {
        opacity: 1;
        /* opacity: 1; */
    }
}
</style>