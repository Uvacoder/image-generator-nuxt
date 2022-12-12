<template>
  <div class="w-full flex flex-col items-center">
    <div class="flex gap-x-1 mb-3 w-full">
      <input
        type="text"
        v-model="imageText"
        placeholder="Search for what comes to your mind..."
        class="border-2 p-2 rounded-md w-full"
      />
      <input
        type="button"
        value="Generate"
        class="border-2 p-2 bg-blue-700 text-white rounded-md"
        @click="generateImage"
      />
    </div>
    <div
      class="overflow-auto border-2 w-full rounded-md flex flex-col items-center justify-center p-6"
    >
      <!-- hint -->
      <div
        v-if="showHint"
        class="flex flex-col items-center justify-center opacity-60 gap-y-2"
      >
        <Icon name="material-symbols:imagesmode" class="text-6xl"> </Icon>
        <span class="text-xl">Your Image will appear here</span>
      </div>
      <!-- loading -->
      <div v-show="isLoading" class="animate-pulse">Loading</div>
      <!-- image -->
      <div>
        <img v-if="showImage" :src="image" alt="" srcset="" />
      </div>
      <div v-if="isEmpty" class="">Yakjar awash na</div>
    </div>
  </div>
</template>

<script setup>
let showHint = ref(true);
let showImage = ref(false);
let isLoading = ref(false);
let isEmpty = ref(false);

const imageText = ref("");
let image = ref("");

async function generateImage() {
  const generateWhat = imageText.value;
  isEmpty.value = false;
  image.value = "";
  showImage.value = false;
  showHint.value = false;
  isLoading.value = true;
  const response = await useFetch("http://localhost:5000/api/generateImage", {
    method: "post",
    body: {
      prompt: generateWhat,
    },
    headers: {
      "Content-Type": "application/json",
    },
  });
  isLoading.value = false;

  if (response.data._value["image"]) {
    image.value = response.data._value["image"];
    showImage.value = true;
    console.log(response.data._value["image"]);
  } else {
    showImage.value = false;
    isEmpty.value = true;
  }
}
</script>
