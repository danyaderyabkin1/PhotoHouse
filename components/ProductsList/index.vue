<script setup lang="ts">
import {useCartStore} from "~/store/useCart";
import type {Slide} from "~/types/slide";
const router = useRouter()
const store = useCartStore()
const {addFavorites} = useAddFavorites()
const emit = defineEmits(['update'])
const props = defineProps<{id?:number,title?: string;slides: Slide[];favorites: number[];small?: boolean}>()

const photo = ref<string>('')
const itemId = ref<number>(0)

const {refresh} = await useAsyncData(() => addFavorites(itemId.value), {immediate: false})


const isOpenModal = ref<boolean>(false);
const inFavorite = ref<boolean>(false);
const openModal = (image:string, id:number):void => {
  itemId.value = id
  photo.value = image
  isOpenModal.value = true
}
const loadPhotoToStore = (photo:string, id:number):void => {
  store.initCart(photo, id)
  isOpenModal.value = false
  setTimeout(() => router.push('/'), 400)
}
const addToFavorites = async () => {
  emit('update')
  await refresh()
  inFavorite.value = !inFavorite.value
}

const inFavoriteBtn = computed<number | undefined>(() => {
  return props.favorites.find((el:number) => el === itemId.value)
});
</script>

<template>
  <section class="catalog pb-4">
    <UContainer>
      <div class="flex">
      </div>
      <div class="grid grid-cols-3 gap-2.5 py-1" >
        <div v-for="(slide, idx) in slides">
          <button @click.prevent="openModal(slide.image, slide.id)" class="relative dark:bg-white bg-black/5 rounded-xl w-full h-full">
            <img :class="small ? 'min-h-32' : 'min-h-36'" class="object-contain rounded-xl" :src="slide.image" alt="slide.icon">
<!--            <span v-if="!small" class="absolute h-full w-full flex items-end justify-center font-semibold text-black bottom-0 left-1/2 -translate-x-1/2 rounded-xl catalog__bg">{{slide.title}}</span>-->
          </button>
        </div>
      </div>
      <UModal :ui="{container: 'items-center', background: 'dark:bg-white'}" v-model="isOpenModal">
        <div class="pt-8 mb-4 relative">
          <img class="rounded-xl" :src="photo ? photo : ''" alt="image">
          <UButton :ui="{ padding: {square: 'p-1'}}" @click.prevent="addToFavorites" v-if="inFavoriteBtn !== undefined" class="p-1.5 absolute -bottom-4 left-0 dark:text-red-600"  variant="link" icon="i-material-symbols-light-favorite"></UButton>
          <UButton :ui="{ padding: {square: 'p-1'}}" @click.prevent="addToFavorites" v-else class="p-1.5 absolute -bottom-4 left-0 dark:text-red-600"  variant="link" icon="i-material-symbols-light-favorite-outline"></UButton>
        </div>
        <UButton @click.prevent="loadPhotoToStore(photo, itemId)" :ui="{variant: {solid: 'dark:bg-blue-500 dark:text-white'}}">Выбрать</UButton>
      </UModal>
    </UContainer>
  </section>
</template>

<style scoped>

</style>