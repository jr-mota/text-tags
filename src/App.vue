<template>
  <v-app>
    <v-main>
      <v-container>
        <attraction-item
          v-for="(attraction, idx) of attractions"
          :name="attraction.name"
          :tags="attraction.tags"
          :key="idx"
          class="mt-5"
        />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
  import AttractionItem from "@/components/AttractionItem.vue";

  export default {
    name: "App",

    components: {
      AttractionItem,
    },

    data: () => ({
      attractions: [
        {
          name: "Достопримечательность 1",
          tags: [
            { icon: "star", text: "4,3" },
            { icon: "", text: "Китайская стена" },
            { icon: "", text: "10,3 км" },
            { icon: "clock", text: "6 часов" },
            { icon: "airplane", text: "Самолет" },
          ],
        },
        {
          name: "Достопримечательность 2",
          tags: [
            { icon: "star", text: "1,3" },
            { icon: "", text: "Эйфелева башня" },
            { icon: "", text: "120,1 км" },
          ],
        },
        {
          name: "Достопримечательность 3",
          tags: [
            { icon: "star", text: "5,0" },
            { icon: "", text: "Московский кремль" },
            { icon: "", text: "50,3 км" },
          ],
        },
      ],
    }),
  };
</script>
