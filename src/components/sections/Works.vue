<template>
  <section id="works" class="common-padding mb-20">
    <div class="flex flex-col">
      <h3
        id="selectedWorks"
        v-html="selectedWorks"
        class="heading-1 text-start leading-none font-bold uppercase"
      ></h3>
      <p
        class="heading-1 text-flax-smoke-400 text-opacity-50 hidden w-4/5 text-end font-extrabold sm:block"
      >
        ( {{ selectedWorksProps.length }} )
      </p>

      <div
        id="selected-works-text"
        class="md:column-gap text-flax-smoke-300 mt-[5%] grid grid-cols-12 justify-end opacity-0 lg:grid"
      >
        <p
          class="heading-6 text-flax-smoke-300/85 col-span-4 text-center text-nowrap lg:col-start-2"
        >
          (
          <span class="inline sm:hidden">{{ selectedWorksProps.length }} </span>
          PROJECTS )
        </p>
        <p
          class="heading-4 font-fancy col-span-8 w-full text-balance sm:font-semibold lg:col-span-7"
        >
          Featured client projects that have been meticulously crafted with
          passion and purpose over the years.
        </p>
      </div>
    </div>

    <div
      class="sm:column-gap relative mt-12 grid size-full grid-cols-12 lg:mt-[10%]"
    >
      <div
        class="text-flax-smoke-100 sticky top-12 col-span-5 hidden h-fit w-full overflow-hidden text-[22vw] leading-[0.8] font-semibold md:flex"
      >
        <span class="font-title! relative -tracking-wider">0</span>
        <span
          id="index"
          class="font-title! relative -tracking-wider will-change-transform"
          >{{ index + 1 }}.</span
        >
      </div>
      <aside
        @mouseenter="showCursor"
        @mouseleave="hideCursor"
        class="relative col-span-full flex flex-col space-y-10 md:col-span-7"
      >
        <div
          v-for="(work, i) in selectedWorksProps"
          :key="i"
          class="work-card @container"
        >
          <a class="group" target="_blank" :href="work.url">
            <div
              class="flex-center relative aspect-square overflow-clip rounded-lg"
            >
              <img
                alt="work-background"
                loading="lazy"
                class="absolute size-full object-cover select-none"
                :src="work.imageBg"
              />
              <div
                class="flex-center z-10 aspect-4/3 size-full overflow-clip rounded-lg object-cover"
              >
                <!-- autoplay="false" -->

                <video
                  :src="work.videoSrc"
                  loop
                  muted
                  @error="handleVideoError"
                  type="video/webm"
                  class="work-video size-[80%] rounded-md object-contain"
                ></video>
              </div>
            </div>
            <div>
              <p class="heading-6 font-title! mt-[2%] mb-[1%] leading-none">
                {{ work.category }}
              </p>
              <div class="items-center justify-between sm:flex">
                <h3 class="heading-3 font-title! font-bold uppercase">
                  {{ work.name }}
                </h3>
                <div class="flex gap-1.5 select-none">
                  <p
                    class="border-flax-smoke-300 hover:bg-flax-smoke-300 hover:text-flax-smoke-900 rounded-full border px-4 py-2 transition-[background-color,color] duration-500 ease-in-out"
                    v-for="tag in work.tags"
                    :key="tag"
                  >
                    <span>{{ tag }}</span>
                  </p>
                  <p
                    class="border-flax-smoke-300 bg-flax-smoke-300 text-flax-smoke-900 hover:text-flax-smoke-300 rounded-full border px-4 py-2 transition-[background-color,color] duration-500 ease-in-out hover:bg-transparent"
                  >
                    <span>{{ work.year }}</span>
                  </p>
                </div>
              </div>
            </div>
          </a>
        </div>
      </aside>
    </div>
  </section>
</template>

<script setup lang="ts">
  import { animateSplitText } from '@/animations';
  import { work1, work2, work3, work4 } from '@/assets/videos';
  import { textSplitterIntoChar } from '@/functions';
  import { computed, onBeforeMount, onMounted, ref } from 'vue';
  import gsap from 'gsap';
  import { workBg1, workBg2, workBg3, workBg4 } from '@/assets/images';
  import { useWindowSize } from '@vueuse/core';

  const isSmallScreen = computed(() => {
    return useWindowSize().width.value < 768;
  });
  const index = ref(0);
  const selectedWorks = ref('Selected Projects /');

  const tl = gsap
    .timeline({ defaults: { duration: 0.25 } })
    .to(['#cursor', '#inner'], {
      scale: 1,
      opacity: 1,
    })
    .paused(true);

  const showCursor = () => {
    tl.play();
  };
  const hideCursor = () => {
    tl.reverse();
  };

  const selectedWorksProps = [
    {
      id: 0,
      name: 'Slander',
      category: 'P2P Based WebRTC Platform with AI Abuse Detection. ',
      tags: ['WebRTC', 'AI', 'Socket.IO'],
      videoSrc: work1,
      imageBg: workBg1,
      url: 'https://www.slander.live/',
      year: '2024',
    },
    {
      id: 1,
      name: 'Pharmago',
      category: 'End to End Pharmacy Delivery Application',
      tags: ['Fullstack', 'CRM', 'UI'],
      videoSrc: work2,
      imageBg: workBg2,
      url: 'https://pharmago.in/',
      year: '2025',
    },
    {
      id: 2,
      name: 'Darwix',
      category: 'AI Automation Platform',
      tags: ['AI Automation', 'NextJS', 'ML'],
      videoSrc: work3,
      imageBg: workBg3,
      url: 'https://darwix.vercel.app/',

      year: '2025',
    },
    {
      id: 3,
      name: 'ECell',
      category: 'Bespoke Solution',
      tags: ['NextJS', 'GSAP', 'Framer'],
      videoSrc: work4,
      imageBg: workBg4,
      url: 'http://ecelldtu.in/',
      year: '2023',
    },
  ];

  const handleVideoError = (event: any) => {
    console.error('Video failed to load:', event);
  };

  // Reusable function to handle forward scroll animation
  const createForwardTimeline = (
    index: any,
    i: any,
    selectedWorksProps: any[],
  ) => {
    const tl = gsap.timeline({
      defaults: { duration: 0.3 },
    });

    // Set and move the #index element
    tl.set('#index', {
      yPercent: 100,
      onComplete: () => {
        index.value = Math.min(i, selectedWorksProps.length - 1);
      },
    }).to('#index', {
      yPercent: 0,
      ease: 'power1.inOut',
    });

    return tl;
  };

  // Reusable function to handle backward scroll animation
  const createBackwardTimeline = (index: any, i: any) => {
    const tl = gsap.timeline({ defaults: { duration: 0.3 } });

    // Set and move the #index element
    tl.set('#index', {
      yPercent: -100,
      onComplete: () => {
        index.value = Math.max(i, 0);
      },
    }).to('#index', {
      yPercent: 0,
      duration: 0.3,
      ease: 'power1.inOut',
    });

    return tl;
  };

  const handleIntersection = (entries: IntersectionObserverEntry[]) => {
    entries.forEach((entry) => {
      const video = entry.target as HTMLVideoElement;
      if (entry.isIntersecting) {
        video.play();
      }
    });
  };

  onBeforeMount(() => {
    selectedWorks.value = textSplitterIntoChar('Bespoke Solutions', true);
  });

  onMounted(() => {
    const workVideos = document.querySelectorAll('.work-video');

    // Create the IntersectionObserver
    const observer = new IntersectionObserver(handleIntersection, {
      threshold: 0.25, // Trigger when 25% of the video is visible
    });

    // Observe each video element
    workVideos.forEach((video) => {
      observer.observe(video);
    });

    animateSplitText(
      '#selectedWorks .letters',
      '#selected-works-text',
      0.7,
      0.01,
      0,
    );

    // Apply GSAP animations to each div
    if (!isSmallScreen.value)
      gsap.utils.toArray('.work-card').forEach((div: any, i: any) => {
        gsap.timeline({ defaults: { duration: 0.7 } }).to(div, {
          scrollTrigger: {
            trigger: div,
            // start: 'top 40%',
            start: 'top 25%',
            // end: 'bottom 40%',
            end: 'bottom 25%',
            scrub: 0.01,
            // markers: true,
            onLeaveBack: () => {
              // Backward scroll animation
              if (index.value !== 0) {
                gsap.to('#index', {
                  yPercent: 100,
                  duration: 0.3,
                  ease: 'power4.inOut',
                  onComplete: () => {
                    createBackwardTimeline(index, i - 1);
                  },
                });
              }
            },
          },
          ease: 'power1.inOut',
          onComplete: () => {
            // Forward scroll animation
            if (index.value !== selectedWorksProps.length - 1) {
              gsap.to('#index', {
                yPercent: -100,
                duration: 0.3,
                ease: 'power4.inOut',
                onComplete: () => {
                  createForwardTimeline(index, i + 1, selectedWorksProps);
                },
              });
            }
          },
        });
      });
  });
</script>
