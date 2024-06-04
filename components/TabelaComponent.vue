<template>
  <v-data-table
    :headers="headers"
    :items="items"
    :search="search"
    theme="light"
    height="300"
    @deleteItem="deleteItem"
    @editItem="editItem"
    style="border: solid green 7px;
    border-radius: 6px"
  >
    <template v-slot:item.action="{ item }">
      <v-icon class="me-2" size="small" @click="editItem(item)">
        mdi-pencil
      </v-icon>
      <v-icon size="small" color="error" @click="deleteItem(item)">
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title> {{ titulo }} </v-toolbar-title>
        <v-btn
          style="color: yellow; background-color: brown"
          @click="abrirDialog()"
        >
          New
        </v-btn>
      </v-toolbar>
      <v-text-field
        v-model="search"
        label="Search"
        prepend-inner-icon="mdi-magnify"
        variant="outlined"
        hide-details
        single-line
      ></v-text-field>
    </template>
  </v-data-table>
</template>

<script>
export default {
  name: "TabelaComponent",
  data() {
    return {
      search: "",
    };
  },
  props: {
    titulo: {
      type: String,
    },
    headers: {
      type: Array,
    },
    items: {
      type: Array,
    },
  },
  methods: {
    editItem(item) {
      this.$emit("editou", item);
    },
    deleteItem(item) {
      this.$emit("deletou", item);
    },
    abrirDialog() {
      this.$emit("abrir-dialog");
    },
  },
};
</script>
