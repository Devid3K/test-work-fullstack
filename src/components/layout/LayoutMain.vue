<template>
    <div class="h-screen">
      <div class="bg-black">
        <!-- Content Area -->
        <nav class="pr-8 flex justify-between  gap-4">
          <section id="left-nav" class="w-full mr-10">
            <img :src="backgroundImage" alt="">
            <img src="../../assets/logo.svg" alt="" class="-mt-14 ml-2">
            <div class="flex gap-2 justify-center -mt-10">
              <img src="../../assets/online.svg" alt="" class="-mt-6">
              <span class="whitespace-nowrap text-white">
                <p class="text-center font-bold  text-xl">ด่าน</p>
                <div class="flex gap-4">
                  <p class="text-sky-400 text-md font-bold">2024-06-28 </p>
                  <p class="text-sky-400 text-md font-bold">20:10:23</p>
                </div>

              </span>
            </div>
          </section>
          <section id="right-nav" class="flex whitespace-nowrap text-white justify-center items-center w-1/4 mt-2">
            <p class="mr-2 text-xl">นางสาวธรพร ศรีสมร</p>
            <img src="../../assets/profile.svg" alt="Snow" class="mr-4">

            <n-dropdown trigger="click" :options="options" :render-option="renderOption" @select="handleSelect">
              <n-button class="flex text-white ">
                <img v-if="currentLanguage === 'TH'" src="../../assets/th.png" alt="x" class="w-5 mr-2">
                <img v-else src="../../assets/en.png" alt="x" class="w-5 mr-2">
                <p class="mr-2">{{ currentLanguage }}</p>
                <Icon icon="weui:arrow-filled" class="rotate-90" width="16" height="20" />
              </n-button>
            </n-dropdown>


          </section>
        </nav>

        <main class="flex p-4 overflow-auto h-screen bg-black w-full mt-4">
          <aside class=" min-w-fit border-2  border-sky-500 rounded-2xl p-2 py-8 h-fit mx-6">
            <img src="../../assets/vector-up.svg" alt="" class="m-auto mb-4 ">
            <div v-for="(image, index) in images" :key="index">
              <img :src="getImagePath(image)" alt="" class=" m-auto mt-2 cursor-pointer ">
            </div>
          </aside>

          <slot />

        

        </main>
      </div>
    </div>
</template>


<script>
import { defineComponent, h, ref } from "vue";
import { NTooltip, useMessage, NDropdown, NButton, NIcon } from "naive-ui";
import { Icon } from '@iconify/vue';

import provinces from '../../static/provinces.json'

export default defineComponent({
  name: 'LayoutMain',
  components: {
    NDropdown,
    NButton,
    Icon,
  },
  setup() {
    const message = useMessage();
    const currentLanguage = ref('TH');
    const images = ref([
      'cctv.svg',
      'bar-chart.svg',
      'search.svg',
      'car.svg',
      'setting.svg',
      'add-user.svg'
    ]);
    const backgroundImage = new URL('../../assets/bg-triangle.svg', import.meta.url).href

    const searchKeyword = ref({
      letter: '',
      numberId: '',
      province: 'กรุงเทพมหานคร',
      defaultList: 'Black List',
      agency: '',
      defaultStatus: 'ทุกสถานะ'
    })

    const handleSelect = (key) => {
      currentLanguage.value = key;
      message.info(String(key));
    };
    const getImagePath = (image) => {
      return new URL(`../../assets/${image}`, import.meta.url).href
    }
    return {
      currentLanguage,
      handleSelect,
      images,
      getImagePath,
      backgroundImage,
      provinces,
      searchKeyword,
      renderOption: ({ node, option }) => {
        return h(
          NTooltip,
          { keepAliveOnHover: false, style: { width: "max-content" } },
          {
            trigger: () => [node],
            default: () => option.key
          }
        );
      },
      renderIcon() {
        return h(NIcon, null, {
          default: () => h(Icon, { icon: "mynaui:search", width: 30, height: 34, class: "text-white" })
        });
      },
      options: [
        {
          label: "TH",
          key: "TH",
        },
        {
          label: "EN",
          key: "EN"
        },
      ],
      list: [
        {
          label: "Black List",
          value: "blackList",
        },
        {
          label: "White List",
          value: "whiteList"
        },
      ],
      status: [
        {
          label: "ทุกสถานะ",
          value: "all",
        },
        {
          label: "อยู่ในกระบวนการ",
          value: "active"
        },
      ]
    };
  }
});
</script>