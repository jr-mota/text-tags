<template>
  <div
    class="text-tag-list"
    :class="{ 'text-tag-list_by-width': alignmentByWidth }"
  >
    <template v-for="(item, idx) of tagsWithCircles">
      <v-icon
        v-if="item.circle"
        v-show="!item.hidden"
        :key="idx"
        :ref="'circle-' + idx"
      >
        mdi-circle-small
      </v-icon>

      <text-tag
        v-else
        v-show="!item.hidden"
        :key="idx"
        :icon="item.icon"
        :text="item.text"
        :ref="'tag-' + idx"
      />
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

    data: () => ({
      tagsWithCircles: [],
      allTagAndCircleWidth: [],
    }),

    created() {
      window.addEventListener("load", () => {
        this.saveAllTagAndCircleHref();
        this.handleAdaptiveTags();
      });

      this.fillTagsAndCircles();
    },

    mounted() {
      window.addEventListener("resize", this.handleAdaptiveTags);
    },

    methods: {
      fillTagsAndCircles() {
        for (let i = 0; i < this.tags.length; ++i) {
          const isFirstElem = i === 0;

          if (!isFirstElem)
            this.tagsWithCircles.push({ circle: true, hidden: false });

          this.tagsWithCircles.push({ ...this.tags[i], hidden: false });
        }
      },
      saveAllTagAndCircleHref() {
        for (const refElemKey in this.$refs) {
          const elem = this.$refs[refElemKey][0].$el;

          this.allTagAndCircleWidth.push(elem.clientWidth);
        }
      },
      handleAdaptiveTags() {
        const wrapperElemStyles = window.getComputedStyle(this.$el.parentNode);
        const wrapperElemWidth =
          parseInt(wrapperElemStyles["width"]) -
          parseInt(wrapperElemStyles["padding-left"]) -
          parseInt(wrapperElemStyles["padding-right"]);

        let widthOfTagsAndCircles = 0;

        for (let i = 0; i < this.tagsWithCircles.length; i += 2) {
          const isFirstElem = i === 0;
          const tagWidth = this.allTagAndCircleWidth[i];
          const circleWidth = this.allTagAndCircleWidth[i - 1] || 0;

          widthOfTagsAndCircles += tagWidth + circleWidth;

          if (widthOfTagsAndCircles >= wrapperElemWidth) {
            this.tagsWithCircles[i].hidden = true;

            if (!isFirstElem) this.tagsWithCircles[i - 1].hidden = true;
          } else {
            this.tagsWithCircles[i].hidden = false;

            if (!isFirstElem) this.tagsWithCircles[i - 1].hidden = false;
          }
        }
      },
    },

    computed: {
      alignmentByWidth() {
        return this.alignment === "by width";
      },
    },

    beforeDestroy() {
      window.removeEventListener("resize", this.handleAdaptiveTags);
    },
  };
</script>

<style lang="scss" scoped>
  .text-tag-list {
    width: 100%;

    display: flex;
    justify-content: flex-start; // default left alignment

    white-space: nowrap;

    &_by-width {
      justify-content: space-between;
    }
  }
</style>
