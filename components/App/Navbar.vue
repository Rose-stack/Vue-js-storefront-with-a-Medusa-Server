<template>
  <SfHeader
    :logo="shopLogo"
    :title="shopName"
    active-icon="account"
  >
    <template #navigation>
      <SfHeaderNavigationItem
        v-for="(category, key) in navbarLinks"
        :key="`sf-header-navigation-item-${key}`"
        :link="`${category.link}`"
        :label="category.title"
      />
    </template>
  </SfHeader>
</template>
<script>
import { SfHeader } from "@storefront-ui/vue";
export default {
  name: "Default",
  components: {
    SfHeader,
  },
  data() {
    return {
      shopName: "My Store App",
      shopLogo: "/logo.svg",
      navbarLinks: [
        {
          title: "Products",
          link: "/",
        },
      ],
    };
  },
};
</script>
