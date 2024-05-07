<template>
  <div class="container p-5">
    <div class="w-full bg-white rounded-xl p-5 max-sm:p-2">
      <div class="title font-bold leading-5 text-xl mb-2">Фото</div>
      <div class="subtitle text-medium leading-3 text-xs text-grey-1">Первое фото будет на обложке объявления.
        Перетащите, чтобы изменить порядок.
      </div>
      <div class="grid grid-cols-3 max-[930px]:grid-cols-2 gap-5 max-sm:gap-1 mt-4">
        <div v-for="(item, index) in data" :key="index">
          <div
              @dragstart="handleDragStart(index)"
              @dragover.prevent="handleDragOver"
              @drop="handleDrop(index)"
              class="h-full max-h-[260px] relative">
            <div
                v-if="!item.image"
                class="max-h-[360px] text-center bg-stone-1 rounded-lg p-6 max-sm:p-1 border-dashed border-grey-2 border flex justify-center items-center flex-col h-full">
              <img src="../../public/images/uploadImg.svg" alt="uploadImg">
              <h3 class="font-semibold text-base max-sm:text-xs leading-5 mt-5 mb-2">Перетащите изображение сюда</h3>
              <p class="font-medium text-sm max-sm:text-xs  leading-4 text-grey-1">Перетащите свое изображение сюда или
                <span
                    class="text-mainBlue cursor-pointer" @click="selectfiles(index)">выберите</span></p>
              <input name="file" class="file hidden" type="file" ref="input" @change="onFileSelect"/>
            </div>
            <div v-else class="h-full">
              <div class="absolute top-3 right-3 flex gap-2">
                <div
                    class="rounded-full p-2 bg-blur/30 z-10 border border-white/9  transition-all duration-300 cursor-pointer">
                  <img src="../../public/images/burger.svg" alt="burger"></div>
                <div
                    class="rounded-full p-2 !bg-blur/30 z-10 border border-white/9 transition-all duration-300 cursor-pointer"
                    @click="moveTrash(index)"><img src="../../public/images/trash.svg" alt="trash"></div>
              </div>
              <img :src="item.image" alt="image" class="rounded-lg h-full w-full"></div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>
<script setup>
import {ref} from 'vue'

const data = ref([
  {
    id: 1,
    image: "./public/images/car1.svg"
  },
  {
    id: 2,
    image: "./public/images/car2.svg"
  },
  {
    id: 3,
    image: "../public/images/car3.svg"
  },
  {
    id: 4,
    image: "/images/car4.svg"
  },
  {
    id: 5,
    image: ""
  },
  {
    id: 6,
    image: ""
  }
])


let newDrag;
let remDrag;

function rmHandleDragStart(i) {
  remDrag = i;
}

function handleDragStart(i) {
  newDrag = i;
}

const handleDragOver = (event) => {
  event.preventDefault();
};

function handleDrop(i) {
  let item = data.value[i];
  data.value[i] = data.value[newDrag];
  data.value[newDrag] = item;
}

function rmHandleDrop(i) {
  console.log("item dropped");
  let remItem = removedList.value[i];
  removedList.value[i] = removedList.value[remDrag];
  removedList.value[remDrag] = remItem;
}

const input = ref(null)
const dataId = ref(null)

function selectfiles(i) {
  dataId.value = i
  input.value[0].click()
}

let url = ref(null)

function onFileSelect(event) {
  const files = event.target.files
  url.value = URL.createObjectURL(files[0])
  data.value[dataId.value].image = url.value
  let full = data.value.every(item => item.image)
  console.log(full)
  if (full) {
    data.value.push({})
    data.value[data.value.length - 1].id = data.value[data.value.length]
    data.value[data.value.length - 1].image = ""
  }
}

function moveTrash(id) {
  data.value=data.value.filter(item => item!== data.value[id])
  data.value.push({})
  data.value[data.value.length - 1].id = data.value[data.value.length]
  data.value[data.value.length - 1].image = ""
}

</script>