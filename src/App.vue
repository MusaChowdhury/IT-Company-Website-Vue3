<template>

  <div class="flex flex-col h-screen">
    <navbar :menu_items=menu_data phone_number="+00000" :company_name=company_name email="dummy@dummy" />


    <div class='flex-grow bg-white-300 dark:bg-gray-500'>

      <home v-if="current_option.state === 'home'" :data=home_data />

      <services v-if="current_option.state === 'services'" :data=development_data />



      <pricing v-if="current_option.state === 'pricing'" :data=pricing_data :activate=pricing_menu />


      <about v-if="current_option.state === 'about'" class="flex grow justify-center bg-white  w-full p-10"/>
        
      











    </div>



    <foot :company_name=company_name fb_link="https://www.facebook.com/" git_link="https://github.com/MusaChowdhury/IT-Company-Website-Vue3" />

  </div>
</template>









<script setup>

import { reactive } from 'vue'
import navbar from "./views/navbar.vue"
import foot from "./views/foot.vue"
import home from "./views/home.vue"
import services from './views/services.vue'
import pricing from "./views/pricing.vue"
import about from "./views/about.vue"



const company_name = "Company"

let current_option = reactive({ state: 'home' })
let pricing_menu = NaN


let home_data = {
  "desktop": {
    title: "Dedicated Desktop Application For Your Business?",
    description: "Standalone or cloud-based, we can provide solution for our clients. We develop application which is not only cross-platform also cost effective. We have expertise in technologies like JavaFx, Dot Net etc.",
  },
  "web_back": {
    title: "Database Solution That is Future Proof & Scalable.",
    description: "Most relatable, robust and scalable backend frameworks are our first choice when planning solutions. We have years experience in proven technologies like Django, Laravel and Spring Boot."
  },
  "mobile": {
    title: "Want Dedicated and Customized Android & iOS App?",
    description: "We provide services to build apps according to your requirements. We build for iOS and Android both for the same cost. Trust us to solve your problems."
  },

}






let development_data = {

  heading: {
    title: 'Title',
    description: 'Description',

  },
  features: {
    seo: {
      title: "SEO (Search Engine Optimization)",
      description: "We offer SEO services to improve website visibility  & ranking. Our team uses latest techniques to drive organic traffic. Services include keyword research, on-page optimization, link building, content optimization & local SEO. Goal is to deliver measurable results & increase clients' online presence."
    },
    repair: {
      title: "Maintenance / Upgrade",
      description: "We specialize in maintaining, fixing, upgrading, and scaling software for businesses. Our team of experts can troubleshoot and resolve software issues quickly. We offer maintenance, repair, upgrading and scaling services to ensure software is always running smoothly and up to date."
    },
    web: {
      title: "Web Development",
      description: "We offer web development using Vue3, React, Django, PHP, and Laravel to create efficient websites. Our proficiency in multiple frameworks enables us to fulfill various client needs with tailored solutions."
    },
    pbx: {
      title: "IP Telephony",
      description: "We provide PBX (Private Branch Exchange) setup and configuration services for businesses of all sizes. Our  experts has  knowledge and experience in configuring and setting up PBX systems to meet the specific needs of our clients. We help businesses improve their communication efficiency and reduce costs."
    }
    ,
    desktop:
    {
      title: "Desktop Application",
      description: "We specialize in developing robust desktop software using a variety of technologies such as JavaFX, Dot Net, Qt, and more. Whether your business is in finance, healthcare, or logistics, we are well-suited to provide you with the software solutions you need to run on desktop devices."
    },
    phone:
    {
      title: "Mobile App (iOS / Android)",
      description: "We specializes in the development of high-performance and efficient mobile applications for both Android and iOS platforms. Utilizing web architecture and scalable technologies, our team of highly skilled developers are able to design user-friendly applications that can accommodate a substantial user base. "
    }

  }
}

let menu_data = {
  "Home": current_option_initiate,
  "Services": current_option_initiate,
  "About": current_option_initiate,
  "Pricing": {

    Development: pricing_menu_initiate, // Name is very important, The name must match with other pricing menu options
    Seo: pricing_menu_initiate,
    "Telephony" : pricing_menu_initiate

  },

}



let pricing_data = {

  "development": [{
    title: "Web Development",
    description: "Starting From",
    money: "$700",
    features: ["Single/Multi Page Architecture", "Scalable Backend", " Beautiful UI/UX ", "Customizable"]

  },
  {
    title: "Mobile App",
    description: "Starting From",
    money: "$899",
    features: ["Both iOS & Android ", "Advance Security ", "Optimized UI", "Customers Tracking", "Free Deployment on Store"]

  },
  {
    title: "Desktop Application",
    description: "Starting From",
    money: "$499",
    features: ["Cloud/Local Database", "Cross Compatible (Linux/Windows)", "Optimized", "Advance UI"]

  },
  ],

  "seo": [
    {
      description: "Search engine optimization for small businesses and individuals who want to improve their search engine rankings and visibility.\nThe package typically includes keyword research, on-page optimization, link building, advanced strategies such as\nlocal SEO, technical SEO and content marketing, as well as monthly reporting.  "
    }

  ], 

  "telephony" : [
    {
      title: "Basic PBX (One-Time)" ,
      description: "Starting From ",
      money: "$1000",
      features: ["On-premises", "Add-on options available", "Scalable to grow with business",]
    },
    {
      title: "Basic PBX (Cloud)",
      description: "Starting From (Monthly)",
      money: "$50",
      features: ["Cloud", "Add-on options available", "Scalable to grow with business","Easy to set up and use"]
    },
    {
      title: "Advance PBX (One-Time)" ,
      description: "Starting From ",
      money: "$1500",
      features: ["On-premises", "Advanced features, such as call routing and voicemail", "Integration with CRM and other software","Integration With AI"]
    },
    {
      title: "Advance PBX (Cloud)",
      description: "Starting From (Monthly)",
      money: "$70",
      features: ["Cloud","Integration With AI", "Scalable to grow with business","Easy to set up and use"]
    },
    {
      title: "Basic PBX (One-Time)" ,
      description: "Starting From ",
      money: "$1000",
      features: ["On-premises", "Add-on options available", "Scalable to grow with business",]
    },
    {
      title: "Basic PBX (Cloud)",
      description: "Starting From (Monthly)",
      money: "$50",
      features: ["Cloud", "Add-on options available", "Scalable to grow with business","Easy to set up and use"]
    },
  ]


}










function handel_menu_change(target) {
  current_option.state = 'none'
  setTimeout(() => current_option.state = target, 100)
}

function pricing_menu_initiate(selected) {
  
  pricing_menu = selected
  handel_menu_change('pricing')
  // console.log(selected)
}

function current_option_initiate(current) {
  // handel_fade(current)
  handel_menu_change(current)
  
  
}
/// Fro pre loading images


let price_background_image = new Image()
price_background_image.src = '/contact.webp' 




</script>































<style scoped>


</style>