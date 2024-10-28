<script setup lang="ts">
import itemsTree from '@/task_json.json'

const locales = ref(['ru', 'en', 'fr'])

const currentLocale = ref('ru')

</script>

<template>
 <div class="p-10">
  <select
    v-model="currentLocale"
    name="locale"
    class="border p-2 mb-2"
    id="locale">
   <option value="">Выберите язык</option>
   <option
     v-for="(locale, index) of locales"
     :key="index"
     :value="locale">
    {{locale}}
   </option>
  </select>

  <TreeComponent
    v-for="item in itemsTree"
    :key="item.id"
    :item="item"
    :locale="currentLocale"
    :tree="itemsTree"
  />
 </div>
</template>