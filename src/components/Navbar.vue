<script>
import { appleImg,searchImg,bagImg } from "../utils/index.js";
import {navLists} from "../constants/index.js";
export default {
  name: "Navbar",
  data(){
    return {
        appleImg,searchImg,bagImg,navLists
    }
  }
};
</script>

<template>
  <header class="w-full py-5 sm:px-10 px-5 flex justify-between items-center">
    <nav class="flex w-full screen-max-width">
      
      <img :src="appleImg" alt="Apple" width="14" height="18" />

      <div
      v-for="nav in navLists"
      :key="nav"
      class="flex flex-1 justify-center max-sm:hidden"
      >
        <div class="px-5 text-sm cursor-pointer text-gray hover:text-white transition-all">
            {{ nav }}
        </div>
        
      </div>

      <div class="flex items-baseline gap-7 max-sm:justify-end max-sm:flex-1">
        <img :src="searchImg" alt="search" height="18" width="18"/>
        <img :src="bagImg" alt="bag" height="18" width="18"/>
      </div>
    </nav>
  </header>
</template>
