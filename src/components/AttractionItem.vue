<template>
  <v-card>
    <v-card-title>{{ name }}</v-card-title>
    <v-card-text>
      <text-tag-list :tags="tags" :alignment="tagsAlignment" />
    </v-card-text>
  </v-card>
</template>

<script>
  import TextTagList from "./TextTagList.vue";

  export default {
    components: { TextTagList },

    props: {
      name: {
        type: String,
        default: "",
      },
      tags: {
        type: Array,
        default: () => [],
      },
    },

    data: () => ({
      tagsAlignment: "left", // or "by width"
    }),
  };
</script>
