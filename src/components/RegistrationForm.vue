<template>
  <v-container fluid>
    <v-form ref="form">
      <!-- First row -->
      <v-layout row>
        <v-text-field label="Фамилия" name="surname" :rules="[rules.required, rules.oneWord]" />
        <v-text-field label="Имя" name="name" :rules="[rules.required, rules.oneWord]" />
        <v-text-field label="Отчество" name="patronymic" :rules="[rules.required, rules.oneWord]" />
      </v-layout>
      <!-- Second row -->
      <v-layout row>
        <v-select :items="passportOptions" name="passportType" label="Тип паспорта" clearable v-model="passportType" />
        <v-text-field
          label="Серия паспорта"
          name="passportSeries"
          mask="####"
          v-if="passportType == 'Паспорт РФ'"
          key="russian-series"
          :rules="[rules.required]"
        />
        <v-text-field
          label="Номер паспорта"
          name="passportNumber"
          mask="######"
          v-if="passportType == 'Паспорт РФ'"
          key="russian-number"
          :rules="[rules.required]"
        />
        <v-text-field
          label="Серия паспорта"
          name="passportSeries"
          v-if="passportType == 'Паспорт иностранного гражданина'"
          key="foreign-series"
          :rules="[rules.required]"
        />
        <v-text-field
          label="Номер паспорта"
          name="passportNumber"
          v-if="passportType == 'Паспорт иностранного гражданина'"
          key="foreign-number"
          :rules="[rules.required]"
        />
        <v-spacer v-if="!passportType" />
        <v-spacer v-if="!passportType" />
      </v-layout>
      <!-- Third row -->
      <v-layout row>
        <v-text-field label="Email" name="email" :rules="[rules.email]" />
        <v-text-field label="Телефон" name="phone" mask="+7 (###) ###-####" :rules="[rules.required]" />
        <DatePicker
          label="Дата рождения" name="birthdayDate" :max="computeMaxBirthdayDate()" :rules="[rules.required]"
        />
      </v-layout>
      <!-- Fourth row -->
      <v-layout row>
        <v-select :items="groupsOptions" label="Группы пользователя" name="groups" multiple :rules="[rules.required]" />
        <DatePicker label="Дата трудоустройства" name="employmentDate" :rules="[rules.required]" />
        <v-select :items="statusOptions" label="Статус" name="status" :rules="[rules.required]" />
      </v-layout>
      <v-layout>
        <v-btn @click="clearForm">Очистить</v-btn>
        <v-btn color="primary" @click="saveForm">Сохранить и продолжить редактирование</v-btn>
      </v-layout>
    </v-form>
    <PopupMessage v-model="successMessage">Пользователь успешно создан</PopupMessage>
  </v-container>
</template>

<script>
import DatePicker from "./DatePicker";
import PopupMessage from "./PopupMessage";

export default {
  name: "RegistrationForm",
  data: () => ({
    passportOptions: [
      "Паспорт РФ",
      "Паспорт иностранного гражданина"
    ],
    groupsOptions: [
      "Суперпользователь",
      "Модератор",
      "Менеджер",
      "PM",
      "BPM"
    ],
    statusOptions: [
      "Активен",
      "Собеседование",
      "Отпуск",
      "Отгул",
      "Командировка"
    ],
    rules: {
      required: (value) => {
        if (value === null || value === undefined || value.length == 0) {
          return "Поле обязательно для заполнения";
        }

        return true;
      },
      oneWord: (value) => {
        if (value !== undefined && value.length > 0) {
          if (!/^[а-яА-ЯёЁa-zA-Z\s]+$/.test(value)) {
            return "Допускаются только символы русского и английского алфавитов";
          }

          if (value.includes(" ")) {
            return "Пробелы недопустимы";
          }
        }

        return true;
      },
      email: (value) => {
        if (value !== undefined && value.length > 0) {
          if (!/^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/.test(value)) {
            return "Почтовый адрес введён неверно";
          }
        }

        return true;
      }
    },
    passportType: null,
    successMessage: false
  }),
  methods: {
    computeMaxBirthdayDate() {
      let date = new Date();
      date.setFullYear(date.getFullYear() - 18);
      let ISOFormattedDate = date.toISOString()
      return ISOFormattedDate.slice(0, ISOFormattedDate.indexOf("T"));
    },
    clearForm() {
      this.$refs.form.reset();
    },
    saveForm() {
      if (this.$refs.form.validate()) {
        this.successMessage = true;
      }
    }
  },
  components: {DatePicker, PopupMessage}
}
</script>

<!--  Style is intentionally unscoped -->
<style>
.layout.row > div {
  flex-basis: 100%; /* Force flex columns to have equal width */
  margin: 0 10px;
}

.v-text-field {
  margin-top: 0px !important;
}
</style>
