<script setup lang="ts">
import { useSidebarStore } from '@/stores/sidebarStore'
import DesktopSidebar from '../components/Sidebar/desktop-sidebar.vue'
import MobileSidebar from '../components/Sidebar/mobile-sidebar.vue'
import { Card } from '../components/ui/card'
import { useRoute } from 'vue-router'
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from '@/core/components/ui/collapsible'
import { ref } from 'vue'

// GET SIDEBAR HEADING
const sidebarStore = useSidebarStore()
const route = useRoute()
const heading = sidebarStore.getServiceHeading(route.path)

// NAVIGATION ROUTES
const navigation = [
  { name: 'Dashboard', href: '#', icon: 'grid_view', current: true },
  { name: 'Manage Services', href: '#', icon: 'bookmark_border', current: false },
]

// GET PLATFORM SERVICE LINKS
const isPlatformServicesCollapsibleOpen = ref(false)
</script>

<template>
  <div>
    <MobileSidebar />

    <DesktopSidebar>
      <li>
        <h1 class="text-xs font-bold uppercase text-primary/75">{{ heading }}</h1>
      </li>
      <li>
        <ul role="list" class="-mx-2 space-y-1">
          <li v-for="item in navigation" :key="item.name">
            <RouterLink
              :to="item.href"
              class="group flex items-center gap-x-3 rounded-md p-2 text-sm/6 font-semibold hover:bg-primary/5"
            >
              <i class="material-icons text-2xl">{{ item.icon }}</i>
              {{ item.name }}
            </RouterLink>
          </li>
        </ul>
      </li>
      <Collapsible v-model:open="isPlatformServicesCollapsibleOpen">
        <CollapsibleTrigger>Pinned Services</CollapsibleTrigger>
        <CollapsibleContent>
          Yes. Free to use for personal and commercial projects. No attribution required.
        </CollapsibleContent>
      </Collapsible>
    </DesktopSidebar>

    <div class="lg:pl-72">
      <header class="p-4">
        <Card> qewqewq </Card>
      </header>

      <slot />
    </div>
  </div>
</template>
