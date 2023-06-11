<template>
    <section class="text-gray-600 body-font">
        <div class="container px-5 py-24 mx-auto">
            <div class="lg:w-2/3 flex flex-col sm:flex-row sm:items-center items-start mx-auto">
                <textarea v-model="prompt" type="text" rows="5" class="flex-grow rounded-3xl bg-slate-50 mr-5 p-2 sm:pr-16 text-2xl font-medium title-font text-gray-900"/>
                <button @click="handleClick" class="flex-shrink-0 border-2 text-white bg-indigo-500 border-0 py-2 px-8 focus:outline-none hover:bg-indigo-600 rounded text-lg mt-10 sm:mt-0">ASK</button>
            </div>
        </div>
    </section>
    <section class="text-gray-600 body-font">
        <div class="container px-5 mx-auto">
          <div class="flex flex-wrap -m-4">
            <div v-for="item in items" :key="item.prompt" class="xl:w-1/3 md:w-1/2 p-4">
              <div class="border border-gray-200 p-6 rounded-lg">
                <h2 class="text-lg text-gray-900 font-medium title-font mb-2">{{ item.prompt }}</h2>
                <p class="leading-relaxed text-base">{{ item.response }}</p>
              </div>
            </div>
          </div>
        </div>
      </section>
</template>
<script>
import { defineComponent } from "vue";
export default defineComponent({
  data() {
    return {
        prompt: '',
        generatedText: '',
        items: []
    };
  },
  methods: {
    async handleClick() {
        if (this.prompt === '') return;
        const {data} = await useFetch('/api/generate', {
            method: 'POST',
            body: {
                prompt: this.prompt
            }
        });

        this.generatedText = data.value.choices[0].text

        this.items.push({
            prompt: this.prompt,
            response: this.generatedText
        })

        // 初期化
        this.prompt = ''
        this.generatedText = ''
    },
  },
});
</script>