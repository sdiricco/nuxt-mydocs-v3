<template>

  <div class="relative flex flex-col items-center justify-center w-full my-auto"
    style="height: calc(100vh - 96px - 56px);">
    <div class="absolute inset-0 z-0 flex w-full">
      <Three />
    </div>

    <section class="relative z-10  mx-auto flex flex-col items-center gap-2">
      <NuxtLink v-if="announcement" :to="announcement.to" :target="announcement.target"
        class="inline-flex items-center rounded-lg bg-muted px-3 py-1 text-sm font-medium">
        <template v-if="announcement.icon">
          <Icon :name="announcement.icon" size="16" />
          <UiSeparator class="mx-2 h-4" orientation="vertical" />
        </template>
        <span class="sm:hidden">{{ announcement.title }}</span>
        <span class="hidden sm:inline">
          {{ announcement.title }}
        </span>
        <Icon name="lucide:arrow-right" class="ml-1 h-4 w-4" />
      </NuxtLink>



      <div class="text-center flex flex-col items-center justify-center">
        <h1 class=" text-3xl font-bold leading-tight tracking-tighter md:text-6xl lg:leading-[1.1]">
          <ContentSlot :use="$slots.title" unwrap="p" />
        </h1>
        <span class="text-lg text-muted-foreground sm:text-xl block mt-4">
          <ContentSlot :use="$slots.description" unwrap="p" />
        </span>

        <section class="flex w-full items-center justify-center space-x-4 py-4 md:pb-10">
          <NuxtLink v-for="(action, i) in actions" :key="i" :to="action.to" :target="action.target">
            <UiButton :variant="action.variant">
              <Icon v-if="action.leftIcon" :name="action.leftIcon" class="mr-1" />
              {{ action.name }}
              <Icon v-if="action.rightIcon" :name="action.rightIcon" class="ml-1" />
            </UiButton>
          </NuxtLink>
        </section>

      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  announcement?: {
    to?: string;
    target?: string;
    icon?: string;
    title: string;
  };
  actions: [{
    name: string;
    leftIcon?: string;
    rightIcon?: string;
    variant?: 'default' | 'link' | 'destructive' | 'outline' | 'secondary' | 'ghost';
    to: string;
    target?: string;
  }];
}>();
</script>
