<template>
  <div
    class="text-tag-list"
    :class="{ 'text-tag-list_by-width': alignment === 'by width' }"
  >
    <template v-for="(item, idx) of tagsWithCircles">
      <v-icon
        v-if="item.circle && !item.hidden"
        :ref="'circle-' + idx"
        :key="idx"
      >
        mdi-circle-small
      </v-icon>
      <text-tag
        v-else-if="!item.hidden"
        :ref="'tag-' + idx"
        :key="idx"
        :icon="item.icon"
        :text="item.text"
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
      this.fillTagsAndCirclesToArr();
    },

    mounted() {
      this.saveAllTagAndCircleWidth();
      this.handleAdaptiveOfTags();

      window.addEventListener("resize", this.handleAdaptiveOfTags);
    },

    methods: {
      fillTagsAndCirclesToArr() {
        for (let i = 0; i < this.tags.length; ++i) {
          if (i > 0) this.tagsWithCircles.push({ circle: true, hidden: false });

          this.tagsWithCircles.push({ ...this.tags[i], hidden: false });
        }
      },
      saveAllTagAndCircleWidth() {
        for (const elemKey in this.$refs) {
          this.allTagAndCircleWidth.push(
            this.$refs[elemKey][0].$el.clientWidth
          );
        }
      },
      handleAdaptiveOfTags() {
        const wrapperElem = this.$el.parentNode;
        const wrapperElemStyles = window.getComputedStyle(wrapperElem);

        const wrapperElemWidth =
          parseInt(wrapperElemStyles["width"]) -
          parseInt(wrapperElemStyles["padding-left"]) -
          parseInt(wrapperElemStyles["padding-right"]);

        let widthOfElems = 0;

        for (let i = 0; i < this.tagsWithCircles.length; i += 2) {
          const isFirstElem = i === 0;

          const tagWidth = this.allTagAndCircleWidth[i];
          let circleWidth = this.allTagAndCircleWidth[i - 1];

          if (isFirstElem) {
            circleWidth = 0;
          }

          widthOfElems += tagWidth + circleWidth;

          if (widthOfElems > wrapperElemWidth) {
            this.tagsWithCircles[i].hidden = true;

            if (!isFirstElem) this.tagsWithCircles[i - 1].hidden = true;
          } else {
            this.tagsWithCircles[i].hidden = false;

            if (!isFirstElem) this.tagsWithCircles[i - 1].hidden = false;
          }
        }
      },
    },

    beforeDestroy() {
      window.removeEventListener("resize", this.handleAdaptiveOfTags);
    },
  };
</script>

<style lang="scss" scoped>
  .text-tag-list {
    width: 100%;

    display: flex;
    justify-content: flex-start;

    white-space: nowrap;

    &_by-width {
      justify-content: space-between;
    }
  }
</style>
