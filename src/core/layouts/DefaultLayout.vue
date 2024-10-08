<script setup lang="ts">
import ServicesModal from '../components/services-modal.vue'
import DesktopSidebar from '../components/sidebar/desktop-sidebar.vue'
import { Avatar, AvatarImage, AvatarFallback } from '../components/ui/avatar'
import { Card } from '../components/ui/card'
import { uiHelpers } from '../utils/ui-helper'
import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from '@/core/components/ui/breadcrumb'
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from '@/core/components/ui/collapsible'
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from '@/core/components/ui/dropdown-menu'
import { useServicesStore } from '@/stores/servicesStore'
import { useSidebarStore } from '@/stores/sidebarStore'
import { computed, ref } from 'vue'
import { useRoute } from 'vue-router'

const sidebarStore = useSidebarStore()
const route = useRoute()
const heading = sidebarStore.getServiceHeading(route.path)

const isPlatformServicesCollapsibleOpen = ref(true)

const isServicesModalOpen = ref(false)

const servicesStore = useServicesStore()
const services = servicesStore.getServiceLinks(route.path)
const pinnedServices = computed(() => [...services].filter((service) => service[1].pinned))

function toggleServicesModal() {
  isServicesModalOpen.value = !isServicesModalOpen.value
}

const breadcrumbs = route.path.split('/').filter(Boolean)
const parentRoute = breadcrumbs[0]
</script>

<template>
  <div>
    <!-- <MobileSidebar /> -->

    <DesktopSidebar>
      <!-- heading -->
      <li>
        <h1 class="text-xs font-bold uppercase text-primary/75">{{ heading }}</h1>
      </li>

      <!-- navigation routes -->
      <li>
        <ul role="list" class="-mx-2">
          <li>
            <RouterLink
              :to="{ name: parentRoute }"
              class="group flex items-center gap-x-3 rounded-md p-2 text-sm/6 font-semibold hover:bg-primary/5"
            >
              <i class="material-icons text-xl">grid_view</i>
              Dashboard
            </RouterLink>
          </li>
          <li>
            <button
              class="group flex w-full items-center gap-x-3 rounded-md p-2 text-sm/6 font-semibold hover:bg-primary/5"
              @click="toggleServicesModal"
            >
              <i class="material-icons text-xl">bookmark_border</i>
              Manage Services
            </button>
          </li>
        </ul>
      </li>

      <!-- pinned services -->
      <Collapsible v-model:open="isPlatformServicesCollapsibleOpen" class="flex flex-col gap-y-1">
        <CollapsibleTrigger
          class="flex w-full items-center justify-between text-xs font-bold uppercase text-primary/75"
        >
          Pinned Services
          <i
            :class="[
              'material-icons text-2xl transition-transform',
              isPlatformServicesCollapsibleOpen && 'rotate-180',
            ]"
          >
            arrow_drop_down
          </i>
        </CollapsibleTrigger>
        <CollapsibleContent as="ul" class="-mx-2">
          <li v-for="[key, value] in pinnedServices" :key="key">
            <RouterLink
              :to="key"
              class="grid grid-cols-[20px_1fr_20px] items-center gap-x-3 rounded-md p-2 text-sm/6 font-semibold hover:bg-primary/5"
            >
              <i :class="['bx text-xl', value.icon]"></i>
              <span>
                {{ value.label }}
              </span>
              <button
                class="grid place-content-center"
                @click.prevent="servicesStore.toggleServicePinnedStatus(route.path, key)"
              >
                <i class="material-icons text-xl">bookmark</i>
              </button>
            </RouterLink>
          </li>
        </CollapsibleContent>
      </Collapsible>
    </DesktopSidebar>

    <!-- services modal -->
    <ServicesModal v-model:open="isServicesModalOpen" />

    <div class="lg:pl-72">
      <!-- header -->
      <header class="p-4">
        <Card class="flex items-center gap-x-2 bg-primary/5 px-4 py-2">
          <div class="flex grow flex-col gap-y-1">
            <DropdownMenu>
              <DropdownMenuTrigger class="flex items-center self-start text-xs/4 font-bold">
                Marketing Master IO Page
                <i class="material-icons">arrow_drop_down</i>
              </DropdownMenuTrigger>
              <DropdownMenuContent>
                <DropdownMenuItem>Item #1</DropdownMenuItem>
                <DropdownMenuItem>Item #2</DropdownMenuItem>
                <DropdownMenuItem>Item #3</DropdownMenuItem>
              </DropdownMenuContent>
            </DropdownMenu>
            <Breadcrumb>
              <BreadcrumbList class="text-xs/4">
                <BreadcrumbItem v-for="breadcrumb in breadcrumbs" :key="breadcrumb">
                  <BreadcrumbLink v-if="breadcrumb !== route.name" as-child>
                    <RouterLink :to="{ name: breadcrumb }">
                      {{ uiHelpers.toTitleCase(breadcrumb) }}
                    </RouterLink>
                  </BreadcrumbLink>
                  <BreadcrumbPage v-else>
                    {{ uiHelpers.toTitleCase(breadcrumb) }}
                  </BreadcrumbPage>
                  <BreadcrumbSeparator v-if="breadcrumb !== breadcrumbs[breadcrumbs.length - 1]" />
                </BreadcrumbItem>
              </BreadcrumbList>
            </Breadcrumb>
          </div>
          <button class="grid place-content-center">
            <i class="material-icons text-3xl">keyboard_return</i>
          </button>
          <button class="grid place-content-center">
            <i class="material-icons text-3xl">notifications</i>
          </button>
          <Avatar>
            <AvatarImage src="https://placehold.co/48" />
            <AvatarFallback>UI</AvatarFallback>
          </Avatar>
        </Card>
      </header>

      <!-- main content -->
      <slot />
    </div>
  </div>
</template>
