<script setup lang="ts">
import { SearchRound } from '@vicons/material';
import { codesByCategories } from './http-status-codes.constants';
import { useFuzzySearch } from '@/composable/fuzzySearch';

const search = ref('');

const { searchResult } = useFuzzySearch({
  search,
  data: codesByCategories.flatMap(({ codes, category }) => codes.map(code => ({ ...code, category }))),
  options: {
    keys: [{ name: 'code', weight: 3 }, { name: 'name', weight: 2 }, 'description', 'category'],
  },
});

const codesByCategoryFiltered = computed(() => {
  if (!search.value) {
    return codesByCategories;
  }

  return [{ category: 'Search results', codes: searchResult.value }];
});
</script>

<template>
  <div>
    <n-form-item :show-label="false">
      <n-input
        v-model:value="search"
        placeholder="Search http status..."
        size="large"
        autofocus
        mb-10
        autocomplete="off"
        autocorrect="off"
        autocapitalize="off"
      >
        <template #prefix>
          <n-icon :component="SearchRound" />
        </template>
      </n-input>
    </n-form-item>

    <div v-for="{ codes, category } of codesByCategoryFiltered" :key="category" mb-8>
      <n-h2> {{ category }} </n-h2>

      <c-card v-for="{ code, description, name, type } of codes" :key="code" mb-2>
        <n-text strong block text-lg>
          {{ code }} {{ name }}
        </n-text>
        <n-text block depth="3">
          {{ description }} {{ type !== 'HTTP' ? `For ${type}.` : '' }}
        </n-text>
      </c-card>
    </div>
  </div>
</template>
