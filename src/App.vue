<template>
  <v-app>
    <v-content>
      <RegistrationForm/>
    </v-content>
  </v-app>
</template>

<script>
import RegistrationForm from "./components/RegistrationForm";

export default {
  name: "App",
  components: {
    RegistrationForm
  }
}
</script>
