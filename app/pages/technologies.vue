<script setup lang="ts">
type Event = {
  title: string;
  date: string;
  location: string;
  url?: string;
  category: "Conference" | "Live talk" | "Podcast";
};

const { data: page } = await useAsyncData("technologies", () => {
  return queryCollection("technologies").first();
});
if (!page.value) {
  throw createError({
    statusCode: 404,
    statusMessage: "Page not found",
    fatal: true,
  });
}

useSeoMeta({
  title: page.value?.seo?.title || page.value?.title,
  ogTitle: page.value?.seo?.title || page.value?.title,
  description: page.value?.seo?.description || page.value?.description,
  ogDescription: page.value?.seo?.description || page.value?.description,
});

const { global } = useAppConfig();

const groupedEvents = computed((): Record<Event["category"], Event[]> => {
  const events = page.value?.events || [];
  const grouped: Record<Event["category"], Event[]> = {
    Conference: [],
    "Live talk": [],
    Podcast: [],
  };
  for (const event of events) {
    if (grouped[event.category]) grouped[event.category].push(event);
  }
  return grouped;
});

function formatDate(dateString: string): string {
  return new Date(dateString).toLocaleDateString("en-US", {
    year: "numeric",
    month: "long",
  });
}
</script>

<template>
  <UPage v-if="page">
    <UPageHero
      :title="page.title"
      :description="page.description"
      :ui="{
        title: '!mx-0 text-left',
        description: '!mx-0 text-left',
        links: 'justify-start',
      }"
    >
    </UPageHero>
    <UPageSection
      :ui="{
        container: '!pt-0',
      }"
    >
      <div v-for="(category, index) in page.categories" :key="index">
        <h3>{{ category.title }}</h3>
        <div class="flex flex-wrap gap-2">
          <TechnoCard
            v-for="(item, idx) in category.items"
            :key="item.id || idx"
            :item="item"
            class="w-[25%] min-w-[250px]"
          />
        </div>
      </div>
    </UPageSection>
  </UPage>
</template>
