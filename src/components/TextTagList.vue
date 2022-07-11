<template>
  <div
    class="text-tag-list"
    :class="{ 'text-tag-list_by-width': alignment === 'by width' }"
  >
    <template v-for="(item, idx) of tagsWithCircles">
      <v-icon v-if="item === 'circle'" :key="idx"> mdi-circle-small </v-icon>
      <text-tag v-else :key="idx" :icon="item.icon" :text="item.text" />
    </template>
  </div>
</template>

<script>
  import TextTag from "./TextTag";

  export default {
    components: {
      TextTag,
    },

    props: {
      tags: {
        type: Array,
        default: () => [],
      },
      alignment: {
        type: String,
        default: "left",
      },
    },

    computed: {
      tagsWithCircles() {
        const arr = [];

        for (let i = 0; i < this.tags.length; ++i) {
          if (i > 0) arr.push("circle");

          arr.push(this.tags[i]);
        }

        return arr;
      },
    },
  };
</script>

<style lang="scss" scoped>
  .text-tag-list {
    width: 100%;

    display: flex;
    justify-content: flex-start;

    white-space: nowrap;

    overflow: hidden;

    &_by-width {
      justify-content: space-between;
    }
  }
</style>
