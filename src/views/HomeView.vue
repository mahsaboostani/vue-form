<template>
  <form @submit.prevent="sendForm">
    <BaseSelect
      :options="categories"
      v-model="event.category"
      label="select a category"
    />
    <BaseInput v-model="event.title" label="Title" type="text" />
    <BaseInput v-model="event.description" label="Description" type="text" />
    <BaseInput v-model="event.location" label="Location " type="text" />
    <BaseCheckbox v-model="event.extras.catering" label="Catering" />
    <BaseCheckbox v-model="event.extras.music" label="Music" />
    <BaseGroupRadio v-model="event.pets" name="pets" :options="petOptions" />
    <button type="submit">submit</button>
  </form>
</template>

<script>
//@ is an alias to /src
import axios from "axios";
import BaseInput from "@/components/BaseInput.vue";
import BaseSelect from "@/components/BaseSelect.vue";
import BaseCheckbox from "@/components/BaseCheckbox.vue";
import BaseGroupRadio from "@/components/BaseGroupRadio.vue";

export default {
  name: "HomeView",
  components: {
    BaseInput,
    BaseSelect,
    BaseCheckbox,
    BaseGroupRadio,
  },
  data() {
    return {
      categories: [
        "sustainability",
        "nature",
        "animal welfare",
        "housing",
        "education",
        "food",
        "community",
      ],
      event: {
        id: 2,
        category: "",
        title: "",
        description: "",
        location: "",
        pets: 1,
        extras: {
          catering: false,
          music: false,
        },
      },
      petOptions: [
        { label: "Yes", value: "1" },
        { label: "No", value: "0" },
      ],
    };
  },
  methods: {
    sendForm() {
      axios
        .post(
          "https://my-json-server.typicode.com/mahsaboostani/vue-form/events",
          this.event
        )
        .then(function (response) {
          console.log("Response", response);
        })
        .catch(function (err) {
          console.log("Error", err);
        });
    },
  },
};
</script>
<style>
form {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
