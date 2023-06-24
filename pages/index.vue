<template>
    <section class="body-font">
        <div class="w-screen pt-5">
          <div class="container mx-auto flex justify-center items-center md:p-0">
              <div class="border border-gray-300 p-6 grid grid-cols-1 gap-6 bg-white shadow-lg rounded-lg">
                  <textarea v-model="prompt" type="text" rows="5" class="flex-grow rounded-3xl bg-slate-50 p-2 sm:pr-16 text-2xl font-medium title-font text-gray-900"/>
                  <div class="flex flex-col md:flex-row">
                      <div class="">
                          <select class="border p-2 rounded">
                              <option>GPT-3.5</option>
                              <option>GPT-4</option>
                          </select>
                      </div>
                      <div class="pt-6 md:pt-0 md:pl-6">
                          <select class="border p-2 rounded">
                              <option>Expert</option>
                              <option>Teacher</option>
                              <option>Generalist</option>
                          </select>
                      </div>
                      <div class="pt-6 md:pt-0 md:pl-6">
                          <select class="border p-2 rounded">
                              <option>Generate code</option>
                              <option>Bug</option>
                              <option>Debug</option>
                              <option>Refactor</option>
                              <option>Advice</option>
                              <option>Review</option>
                              <option>Add Comment</option>
                              <option>Name Variable or Function</option>
                          </select>
                      </div>
                  </div>
                  <div class="flex justify-center"><button @click="handleClick" class="p-2 border w-1/4 rounded-md bg-gray-800 text-white">Search</button></div>
              </div>
          </div>
      </div>
    </section>
    <section class="text-gray-600 body-font">
        <div class="container px-5 pt-4 mx-auto">
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
        });

        // Initialization
        this.prompt = '';
        this.generatedText = '';
    },
  },
});
</script>