<template>
  <div :style="bgImage" class="md:w-full md:h-screen bg-gradient-to-r from-amber-200 to-amber-600 flex flex-col">
    <div id="coinmarketcap-widget-marquee"
         coins="1,1027,1839,3890,1975,6636,5426,7278,6535,5805,8000,10603"
         currency="USD"
         show-symbol-logo="true"
         theme="light"
         transparent="false"
    />
    <div class="flex justify-end">
      <div class="border mr-5 mt-5 bg-black/75">
        <i class="fa-solid fa-bars text-6xl text-white  cursor-pointer px-4 py-3" @click="void 0"/>
      </div>
    </div>
    <div class="flex justify-center">
      <div class="text-sm flex gap-3 text-white border-2 bg-black/75 p-8">
        <div class="my-auto">
          <div class="flex gap-2">
            <div class="text-8xl rotate-12 mr-2">HENLO!</div>
            <div class="">
              <div class="flex">
                <div class="text-3xl -ml-32 -mt-2">my name is</div>
                <div class="flex gap-2 ml-auto -mt-5">
                  <a href="https://github.com/SaboneteAssado">
                    <i class="fa-brands fa-github text-4xl"/>
                  </a>
                  <a href="https://www.linkedin.com/in/miguel-ara%C3%BAjo-441337182/">
                    <i class="fa-brands fa-linkedin text-4xl"/>
                  </a>
                </div>
              </div>
              <div>
                <div class="text-5xl">Miguel Araújo</div>
                <div class="flex justify-end">
                  <div class="text-xs mt-0.5 ">but you can call me anytime</div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!--      <div>Crypto enthusiast</div>-->
        <!--      <div>Msc in computer science - Ocaml and automata theory</div>-->
      </div>
    </div>
  </div>
</template>

<script>
import bgImage from "../assets/images/paineis_tiny.jpg"

export default {
  name: 'IndexPage',
  data() {
    return {
      bgImage: {backgroundImage: `url(${bgImage})`},
    }
  }
}
</script>
