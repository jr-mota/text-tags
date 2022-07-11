<template>
  <v-app>
    <v-main>
      <v-container>
        <v-card v-for="(obj, idx) of objects" :key="idx" class="mt-5">
          <v-card-title>{{ obj.name }}</v-card-title>
          <v-card-text>
            <text-tag-list :tags="obj.tags" :alignment="'left'" />
          </v-card-text>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
  import TextTagList from "./components/TextTagList.vue";

  export default {
    name: "App",

    components: { TextTagList },

    data: () => ({
      objects: [
        {
          name: "Достопримечательность 1",
          tags: [
            { icon: "star", text: "4,3" },
            { icon: "", text: "Китайская стена" },
            { icon: "", text: "10,3 км" },
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
