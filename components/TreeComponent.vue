<template>
 <div>
  <button @click="toggleChild" class="bg-gray-300 w-1/4 text-start p-2">
   {{ localeData.cg_name || 'Unnamed' }}
  </button>

  <p class="text-sm text-gray-500">
   {{ breadcrumbs }}
  </p>

  <a :href="localeData.link" target="_blank" class="text-blue-500 underline">
   {{ localeData.link || 'No link available' }}
  </a>

  <div v-if="item.childs && childShown" class="ml-4">
   <TreeComponent
     v-for="child in item.childs"
     :key="child.id"
     :item="child"
     :locale="locale"
     :tree="tree"
   />
  </div>
 </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue';
import type { PropType } from 'vue';

const props = defineProps({
 item: {
  type: Object as PropType<any>,
  required: true,
 },
 locale: {
  type: String as PropType<string>,
  required: true,
 },
 tree: {
  type: Array as PropType<any[]>,
  required: true,
 },
});

const childShown = ref(false);

const toggleChild = () => {
 childShown.value = !childShown.value;
};

const getLocaleData = (localeData: Record<string, any>, locale: string) => {
 return localeData[locale] || Object.values(localeData).find(item => Object.keys(item).length > 0) || {};
};

const localeData = computed(() => getLocaleData(props.item.locale, props.locale));

const getBreadcrumbs = (path: number[], tree: any[], locale: string) => {
 let breadcrumbs: string[] = [];
 const findCategoryById = (id: number, items: any[]): any | undefined => {
  for (let item of items) {
   if (item.id === id) return item;
   if (item.childs) {
    const found = findCategoryById(id, item.childs);
    if (found) return found;
   }
  }
 };
 path.forEach((id) => {
  const category = findCategoryById(id, tree);
  if (category) {
   const localeData = getLocaleData(category.locale, locale);
   breadcrumbs.push(localeData.cg_name || "Unnamed");
  }
 });
 return breadcrumbs.join(' -> ');
};

const breadcrumbs = computed(() => getBreadcrumbs(props.item.path_to_top, props.tree, props.locale));
</script>