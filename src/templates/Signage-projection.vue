<template>
  <Fullscreen class="bg-black overflow-hidden">
    <ClientOnly>
      <div>
        <TinySlider v-bind="tinySliderOptions" class="">
          <div
            class="relative flex flex-col justify-evenly slide one-up"
            v-for="(event, index) in $page.faqs.edges"
            :key="`event-${index}`"
          >
            <img
              v-if="event.node.image"
              class="absolute h-full w-full object-cover"
              :src="
                `${event.node.image.url}?fit=crop&crop=faces,entropy,top&ar=16:9`
              "
            />
            <div class="absolute z-20 bottom-0">
              <h3 class="mt-0 bg-black text-white p-3">
                {{ event.node.question }}
              </h3>
            </div>
          </div>
        </TinySlider>
      </div>
    </ClientOnly>
  </Fullscreen>
</template>
<page-query>
query Faqs {
  faqs: allFaqs(sortBy: "position", order: ASC) {
    edges {
      node {
        answer
				position
        question
        image {
          url
        }
      }
    }
  }
}
</page-query>
<style lang="postcss" scoped>
body.signage-projection {
  height: 100%;
  min-height: 100%;
  main h3 {
    line-height: 1;
    font-size: 9rem;
    strong {
      font-family: HelveticaNowText-ExtraBold;
      font-weight: normal;
      font-style: normal;
    }
  }
  .one-up.slide {
    height: 100vh;
  }
  .tns-liveregion {
    @apply absolute z-0;
    display: none !important;
  }
}
</style>
<script>
import Fullscreen from "~/layouts/Fullscreen";

export default {
  metaInfo: {
    bodyAttrs: {
      class: "custom-body-class"
    }
  },
  components: {
    Fullscreen,
    TinySlider: () => import("vue-tiny-slider")
  },
  name: "Projection",
  data() {
    return {
      tinySliderOptions: {
        nav: false,
        speed: 1000,
        mode: "gallery",
        slideBy: 1,

        autoplay: true,
        controls: false,
        autoplayButtonOutput: false,
        autoplayTimeout: 5000
      }
    };
  }
};
</script>
