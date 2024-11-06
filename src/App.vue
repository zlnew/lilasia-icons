<script setup lang="ts">
import {
  AppBlock,
  AppButton,
  AppDivider,
  AppDropdown,
  DropdownItem,
  DropdownItemGroup,
  FormInput
} from 'lilasia-ui'
import { onMounted, ref } from 'vue'
import Icon from './components/Icon.vue'
import { computed } from 'vue'
import IconMetadata from './api/Icon'

const search = ref()
const filled = ref(false)
const icons = ref<string[]>([])
const filteredIcons = ref<any[]>([])
const isLoading = ref(false)
const limit = ref(100)
const page = ref(1)

const isPrevDisabled = computed(() => page.value <= 1)
const isNextDisabled = computed(() => {
  const totalPages = Math.ceil(icons.value.length / limit.value)
  return page.value >= totalPages
})

onMounted(async () => {
  await fetchIcons()
  paginateIcons()
})

const fetchIcons = async () => {
  isLoading.value = true

  try {
    const iconNames = await IconMetadata.getNames()

    if (iconNames) {
      icons.value = iconNames
    }
  } catch (error) {
    console.error(error)
  } finally {
    isLoading.value = false
  }
}

const paginateIcons = () => {
  filteredIcons.value = icons.value.slice(0, 100)
}

const prev = () => {
  if (page.value > 1) {
    --page.value
    const start = (page.value - 1) * limit.value
    const end = start + limit.value
    filteredIcons.value = icons.value.slice(start, end)
  }
}

const next = () => {
  const totalPages = Math.ceil(icons.value.length / limit.value)
  if (page.value < totalPages) {
    ++page.value
    const start = (page.value - 1) * limit.value
    const end = start + limit.value
    filteredIcons.value = icons.value.slice(start, end)
  }
}

const searchIcon = () => {
  if (!search.value) {
    paginateIcons()
    return
  }

  filteredIcons.value = icons.value.filter((icon) => search.value === icon)
}

const setIconName = (icon: string) => {
  search.value = icon
}
</script>

<template>
  <main class="container mx-auto flex min-h-screen items-center justify-center py-24">
    <section>
      <AppBlock class="md:w-[40rem]">
        <template #header>
          <div class="text-center">
            <h1 class="text-24 font-600 leading-32">Lilasia Icons</h1>
          </div>
        </template>

        <div class="px-24 pb-24">
          <div class="flex items-center gap-8">
            <AppDropdown :text="filled ? 'Filled' : 'Outlined'">
              <DropdownItemGroup>
                <DropdownItem
                  text="Outlined"
                  :icon="!filled ? 'check' : undefined"
                  @click="filled = false"
                />
                <DropdownItem
                  text="Filled"
                  :icon="filled ? 'check' : undefined"
                  @click="filled = true"
                />
              </DropdownItemGroup>
            </AppDropdown>

            <FormInput
              id="icon"
              v-model="search"
              type="search"
              placeholder="Search&hellip;"
              class="w-full"
              required
              @keyup.enter="searchIcon"
            />
          </div>

          <AppDivider class="my-16" />

          <!-- List of icons -->
          <div
            v-if="filteredIcons.length && !isLoading"
            class="grid h-[400px] grid-cols-10 gap-16 overflow-y-auto overflow-x-hidden"
          >
            <AppButton
              v-for="(icon, index) in filteredIcons"
              :key="index"
              color="light"
              :icon="{ name: icon, filled, size: 40 }"
              :title="icon"
              @click="() => setIconName(icon)"
            />
          </div>

          <!-- Not Found -->
          <div
            v-else-if="!filteredIcons.length && !isLoading"
            class="flex h-[400px] items-center justify-center gap-8"
          >
            <Icon name="search_off" />
            Icon not found
          </div>

          <!-- Loading -->
          <div v-else class="flex h-[400px] items-center justify-center gap-8">
            <div class="inline-flex animate-spin">
              <Icon name="progress_activity" />
            </div>
            Loading&hellip;
          </div>
        </div>

        <template v-if="filteredIcons.length >= limit" #footer>
          <div class="flex items-center justify-between gap-8">
            <div class="flex items-center gap-8">
              <AppButton text="Prev" :disabled="isPrevDisabled" @click="prev" />
              <AppButton text="Next" :disabled="isNextDisabled" @click="next" />
            </div>
          </div>
        </template>
      </AppBlock>
    </section>
  </main>
</template>
