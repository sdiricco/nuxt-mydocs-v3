<template>
  <UiBreadcrumb>
    <UiBreadcrumbList>
      <template v-for="(breadcrumb, index) in breadcrumbs" :key="breadcrumb.title">
        <UiBreadcrumbItem>
          <UiBreadcrumbLink
            class="capitalize cursor-pointer"
            :href="breadcrumb.href"
            :class="{ 'text-foreground': index === breadcrumbs.length - 1 }"
            @click="onBreadCrumb(breadcrumb.href)"
          >
            {{ breadcrumb.title }}
          </UiBreadcrumbLink>
        </UiBreadcrumbItem>
        <UiBreadcrumbSeparator v-if="index !== breadcrumbs.length - 1" />
      </template>
    </UiBreadcrumbList>
  </UiBreadcrumb>
</template>

<script setup lang="ts">
const route = useRoute();

interface Item {
  title: string;
  href: string;
}

function generateBreadcrumb(url: string): Item[] {
  const breadcrumbItems: Item[] = [];
  const segments = url.split('/').filter(segment => segment !== ''); // Remove empty segments

  // Construct breadcrumb for each segment
  let href = '';
  for (let i = 0; i < segments.length; i++) {
    const segment = segments[i].replace('.html', '');
    href += `/${segment}`;
    breadcrumbItems.push({ title: segment, href });
  }
  return breadcrumbItems;
}

function onBreadCrumb(href: string) {
  console.log(href)
}

const breadcrumbs = computed(() => generateBreadcrumb(route.path));
</script>
