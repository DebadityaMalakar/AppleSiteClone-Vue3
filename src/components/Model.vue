<script>
import {gsap} from "gsap";
import {onMounted} from "vue";
import ModelView from "./ModelView.vue";
export default {
    name: "Model",
    setup(){
        onMounted(()=>{
            gsap.to("#heading", {y:0,opacity:1})
        })
    }
}
</script>

<template>
    <section class="common-padding">
        <div class="screen-max-width">
            <h1 id="heading" class="section-heading">
                Take a closer look.
            </h1>

            <div class="flex flex-col items-center mt-5">
                <div class="w-full h-[75vh] md:h-[90vh] overflow-hidden relative">

                </div>
            </div>
        </div>
    </section>
</template>