<script setup lang="ts">
import {useCartStore} from "~/store/useCart";
const ready = ref(true)
const store = useCartStore()

const handleFileUpload = async (e) => {
  const target = e.target;
  if (!FileReader) {
    alert('FileReader не поддерживается');
    return;
  }

  const fileReader = new FileReader();
  fileReader.onload = function () {
    store.initCart(fileReader.result)
    photo.value = fileReader.result;
  }

  fileReader.readAsDataURL(target.files[0]);
}
const photo = ref('/assets/images/heroBottom.webp');

watchEffect(() => {
  store.cart ? ready.value = false : ready.value = true
})
</script>

<template>
  <main>
    <section class="relative">
      <UContainer>

        <img v-if="!store.cart" class="w-full h-[55vh] object-cover rounded-xl mb-1.5" src="/assets/images/hero.webp" alt="hero">
        <div class="w-full h-[55vh] mb-1.5 dark:bg-zinc-800 rounded-xl" v-else>
          <img  class="w-full h-full object-contain mb-2" :src="store.cart" alt="hero">
        </div>
        <UButton to="/catalog" class="mb-1.5" >Выбрать в каталоге примеров</UButton>
        <p class="text-center mb-1.5">или</p>
<!--        <div class="w-full h-[33vh]  mb-1.5 overflow-hidden dark:bg-zinc-800 rounded-xl relative">-->
<!--          <label v-if="!store.cartBottom" class="flex items-center justify-center border-2 absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 cursor-pointer z-10 rounded-full w-14 h-14">-->
<!--            <input @change="handleFileUpload" type="file" hidden>-->
<!--            <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">-->
<!--              <path d="M10.8013 10.8018V17.7578H7.21533V10.8018H0.226074V7.18262H7.21533V0.243164H10.8013V7.18262H17.7739V10.8018H10.8013Z" fill="white"/>-->
<!--            </svg>-->
<!--          </label>-->
<!--          <img  v-if="!store.cartBottom" class="w-full h-full rounded-xl opacity-40 object-cover mb-2 cursor-pointer" :src="store.cartBottom ? store.cartBottom : photo" alt="heroBottom">-->
<!--          <img v-else class="w-full h-full object-contain mb-2 cursor-pointer" :src="store.cartBottom ? store.cartBottom : photo" alt="heroBottom">-->
<!--        </div>-->
        <label class="flex mb-3 cursor-pointer">
          <input @change="handleFileUpload" type="file" hidden>
          <UButton  class=" pointer-events-none" >Загрузить фото с одеждой
          </UButton>
        </label>
        <UButton class="sticky bottom-2" :disabled="ready" to="/choise" :class="{'dark:bg-zinc-800': ready, 'dark:text-zinc-500': ready, 'bg-neutral-200': ready, 'text-zinc-400': ready}" >Продолжить</UButton>
      </UContainer>
    </section>
  </main>

</template>

<style scoped>

</style>