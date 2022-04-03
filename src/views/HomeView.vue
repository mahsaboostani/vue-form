<template>
  <form @submit.prevent="onSubmit()">
    <BaseSelect
      :options="categories"
      v-model="category"
      label="select a category"
    />
    <span class="fieldError">{{ errors.category }}</span>
    <fieldset>
      <legend>Name and describe your event</legend>
      <BaseInput v-model="title" label="Title" type="text" />
      <span class="fieldError">{{ errors.title }}</span>
      <BaseInput v-model="description" label="Description" type="text" />
      <span class="fieldError">{{ errors.description }}</span>
    </fieldset>
    <fieldset>
      <legend>Email</legend>
      <BaseInput v-model="email" label="Email" type="email" />
      <span v-show="errors.email" class="fieldError">{{ errors.email }}</span>
    </fieldset>

    <fieldset>
      <legend>Password</legend>
      <BaseInput v-model="password" label="Password" type="password" />
      <span class="fieldError">{{ errors.password }}</span>
    </fieldset>

    <fieldset>
      <legend>Location</legend>
      <BaseInput v-model="location" label="Location" type="text" />
      <span class="fieldError">{{ errors.location }}</span>
    </fieldset>

    <fieldset>
      <legend>pets</legend>
      <BaseGroupRadio v-model="event.pets" name="pets" :options="petOptions" />
      <span class="fieldError">{{ errors.pets }}</span>
    </fieldset>
    <fieldset>
      <legend>extras</legend>
      <BaseCheckbox v-model="event.extras.catering" label="Catering" />
      <span class="fieldError">{{ errors.catering }}</span>
      <BaseCheckbox v-model="event.extras.music" label="Music" />
      <span class="fieldError">{{ errors.music }}</span>
    </fieldset>

    <button type="submit">Submit</button>
  </form>
</template>

<script>
//@ is an alias to /src
import axios from "axios";
import BaseInput from "@/components/BaseInput.vue";
import BaseSelect from "@/components/BaseSelect.vue";
import BaseCheckbox from "@/components/BaseCheckbox.vue";
import BaseGroupRadio from "@/components/BaseGroupRadio.vue";
import { useField, useForm } from "vee-validate";
import * as yup from "yup";

export default {
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
        id: 1,
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
  setup() {
    // email = (value) => {
    //   if (!value) {
    //     return "this field is required";
    //   }
    //   const regex = /^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$/;
    //   if (!regex.test(String(value).toLowerCase())) {
    //     return "please enter a valid email";
    //   }

    //   return true;
    // };
    const validationSchema = yup.object({
      category: yup.string().required(),
      title: yup.string().required("A cool title is required").min(3),
      description: yup.string().required(),
      password: yup.string().required(),
      email: yup.string().email("plese enter valid email").required(),
      location: yup.string(),
      pets: yup.number(),
      catering: yup.boolean(),
      music: yup.boolean(),
    });

    const { handleSubmit, errors } = useForm({
      validationSchema,
      initialValues: {
        pets: 1,
        catering: false,
        music: false,
      },
    });

    const { value: category } = useField("category");
    const { value: title } = useField("title");
    const { value: description } = useField("description");
    const { value: password } = useField("password");
    const { value: email } = useField("email");
    const { value: location } = useField("location");
    const { value: pets } = useField("pets");
    const { value: catering } = useField("catering");
    const { value: music } = useField("music");
    const onSubmit = handleSubmit((values) => {
      console.log("onSubmit", values);
    });
    return {
      email,
      password,
      category,
      title,
      description,
      location,
      pets,
      catering,
      music,
      onSubmit,
      errors,
    };
  },
  name: "HomeView",
  components: {
    BaseInput,
    BaseSelect,
    BaseCheckbox,
    BaseGroupRadio,
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
fieldset {
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 0;
  margin: 0;
  padding: 0;
}
legend {
  font-size: 28px;
  font-weight: 700;
  margin-top: 20px;
}
.fieldError {
  color: crimson;
}
</style>
