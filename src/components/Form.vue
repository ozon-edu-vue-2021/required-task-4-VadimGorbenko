<template>
  <form class="form" @submit.prevent="onSubmit">
    <fieldset class="form__group">
      <legend>Личные данные</legend>
      <label class="form__item form-item">
        <span class="form-item__label">Фамилия</span>
        <input
          type="text"
          name="lastname"
          :class="[
            'form-item__control',
            { 'form-item__control_invalid': lastNameInvalid },
          ]"
          v-model="formData.lastName"
          autocomplete="family-name"
        />
      </label>
      <label class="form__item form-item">
        <span class="form-item__label">Имя</span>
        <input
          type="text"
          name="firstname"
          :class="[
            'form-item__control',
            { 'form-item__control_invalid': firstNameInvalid },
          ]"
          v-model="formData.firstName"
          autocomplete="given-name"
        />
      </label>
      <label class="form__item form-item">
        <span class="form-item__label">Отчество</span>
        <input
          type="text"
          name="middlename"
          :class="[
            'form-item__control',
            { 'form-item__control_invalid': middleNameInvalid },
          ]"
          v-model="formData.middleName"
          autocomplete="additional-name"
        />
      </label>
      <label class="form__item form-item">
        <span class="form-item__label">Дата рождения</span>
        <input
          type="date"
          name="bdate"
          :class="[
            'form-item__control',
            { 'form-item__control_invalid': bdateInvalid },
          ]"
          placeholder="дд.мм.гггг"
          v-model="formData.bdate"
          autocomplete="bday"
        />
      </label>
      <label class="form__item form-item">
        <span class="form-item__label">E-mail</span>
        <input
          type="email"
          name="email"
          :class="[
            'form-item__control',
            { 'form-item__control_invalid': emailInvalid },
          ]"
          v-model="formData.email"
          autocomplete="email"
        />
      </label>
    </fieldset>
    <fieldset class="form__group">
      <legend>Пол</legend>
      <label class="form__item form-item form-item__type_radio">
        <span class="form-item__label">Мужской</span>
        <input
          type="radio"
          name="gender"
          value="1"
          class="form-item__control"
          v-model="formData.gender"
        />
      </label>
      <label class="form__item form-item form-item__type_radio">
        <span class="form-item__label">Женский</span>
        <input
          type="radio"
          name="gender"
          value="0"
          class="form-item__control"
          v-model="formData.gender"
        />
      </label>
    </fieldset>
    <fieldset class="form__group">
      <legend>Паспортные данные</legend>
      <label class="form__item form-item">
        <span class="form-item__label">Гражданство</span>
        <select
          name="citizenship"
          class="form-item__control"
          v-model="formData.citizenship"
        >
          <option
            v-for="citizenship in citizenships"
            :key="citizenship.id"
            :value="citizenship.flag"
          >
            {{ citizenship.nationality }}
          </option>
        </select>
      </label>
      <label
        class="form__item form-item"
        v-if="formData.citizenship && !isRUcitizenship"
      >
        <span class="form-item__label">Фамилия на латинице</span>
        <input
          type="text"
          name="latin_lastname"
          :class="[
            'form-item__control',
            { 'form-item__control_invalid': latinLastNameInvalid },
          ]"
          aria-describedby="latin-fio"
          v-model="formData.latinLastName"
        />
      </label>
      <label
        class="form__item form-item"
        v-if="formData.citizenship && !isRUcitizenship"
      >
        <span class="form-item__label">Имя на латинице</span>
        <input
          type="text"
          name="latin_lastname"
          :class="[
            'form-item__control',
            { 'form-item__control_invalid': latinFirstNameInvalid },
          ]"
          aria-describedby="latin-fio"
          v-model="formData.latinFirstName"
        />
      </label>
      <p
        id="latin-fio"
        class="hint"
        aria-hidden="true"
        v-if="formData.citizenship && !isRUcitizenship"
      >
        Иностранцы заполняют латинскими буквами. Например, Ivanov Ivan
      </p>
      <label
        class="form__item form-item"
        v-if="formData.citizenship && isRUcitizenship"
      >
        <span class="form-item__label">Серия паспорта</span>
        <input
          type="text"
          name="passport_serial"
          :class="[
            'form-item__control',
            { 'form-item__control_invalid': passportSerialInvalid },
          ]"
          v-model="formData.passportSerial"
        />
      </label>
      <label class="form__item form-item" v-if="formData.citizenship">
        <span class="form-item__label">Номер паспорта</span>
        <input
          type="text"
          name="passport_number"
          :class="[
            'form-item__control',
            { 'form-item__control_invalid': passportNumberInvalid },
          ]"
          v-model="formData.passportNumber"
        />
      </label>
      <label
        class="form__item form-item"
        v-if="formData.citizenship && !isRUcitizenship"
      >
        <span class="form-item__label">Страна выдачи</span>
        <select
          name="passport_citizenship"
          class="form-item__control"
          v-model="formData.passportCitizenship"
        >
          <option
            v-for="citizenship in citizenships"
            :key="citizenship.id"
            :value="citizenship.flag"
          >
            {{ citizenship.nationality }}
          </option>
        </select>
      </label>
      <label
        class="form__item form-item"
        v-if="formData.citizenship && !isRUcitizenship"
      >
        <span class="form-item__label">Тип паспорта</span>
        <select
          name="passport_type"
          class="form-item__control"
          v-model="formData.passportType"
        >
          <option v-for="type in passportTypes" :key="type.id" :value="type.id">
            {{ type.type }}
          </option>
        </select>
      </label>
      <label
        class="form__item form-item"
        v-if="formData.citizenship && isRUcitizenship"
      >
        <span class="form-item__label">Дата выдачи</span>
        <input
          type="date"
          name="passport_date"
          class="form-item__control"
          placeholder="дд.мм.гггг"
          v-model="formData.passportDate"
        />
      </label>
    </fieldset>
    <fieldset class="form__group">
      <legend>Меняли ли фамилию или имя</legend>
      <label class="form__item form-item form-item__type_radio">
        <span class="form-item__label">Нет</span>
        <input
          type="radio"
          name="fio_was_changed"
          value="0"
          class="form-item__control"
          v-model="formData.fioWasChanged"
        />
      </label>
      <label class="form__item form-item form-item__type_radio">
        <span class="form-item__label">Да</span>
        <input
          type="radio"
          name="fio_was_changed"
          value="1"
          class="form-item__control"
          v-model="formData.fioWasChanged"
        />
      </label>
      <label class="form__item form-item" v-if="fioWasChanged">
        <span class="form-item__label">Предыдущая фамилия</span>
        <input
          type="text"
          name="prev_firstname"
          :class="[
            'form-item__control',
            { 'form-item__control_invalid': prevLastNameInvalid },
          ]"
          v-model="formData.prevLastName"
        />
      </label>
      <label class="form__item form-item" v-if="fioWasChanged">
        <span class="form-item__label">Предыдущее имя</span>
        <input
          type="text"
          name="prev_lastname"
          :class="[
            'form-item__control',
            { 'form-item__control_invalid': prevFirstNameInvalid },
          ]"
          v-model="formData.prevFirstName"
        />
      </label>
    </fieldset>
    <button type="submit" :disabled="!isFormValid">Отправить</button>
  </form>
</template>

<script>
import citizenships from "@/assets/data/citizenships.json";
import passportTypes from "@/assets/data/passport-types.json";

const CYRILLIC_FIO_PATTERN = /^[А-аЯ-яЁё\s-]*$/;
const LATIN_FIO_PATTERN = /^[A-аZ-z\s-]*$/;
const EMAIL_PATTERN = /^[\w-.]+@([\w-]+\.)+[\w-]{2,4}$/;
const MAX_PASSPORT_SERIAL_LENGTH = 4;
const MAX_RU_PASSPORT_NUMBER_LENGTH = 6;

const CYRILLIC_FIO_VALIDATOR = (value) => CYRILLIC_FIO_PATTERN.test(value);
const LATIN_FIO_VALIDATOR = (value) => LATIN_FIO_PATTERN.test(value);
const EMAIL_VALIDATOR = (value) => EMAIL_PATTERN.test(value);

export default {
  name: "Form",
  data() {
    return {
      citizenships,
      passportTypes,
      formData: {
        firstName: "",
        middleName: "",
        lastName: "",
        bdate: "",
        email: "",
        gender: "",
        citizenship: "",
        passportSerial: "",
        passportNumber: "",
        passportDate: "",
        latinFirstName: "",
        latinLastName: "",
        passportCitizenship: "",
        passportType: "",
        fioWasChanged: false,
        prevFirstName: "",
        prevLastName: "",
      },
    };
  },
  methods: {
    onSubmit() {
      if (this.isFormValid) {
        console.log(this.formData);
      } else {
        alert("Не все поля формы заполнены корректно.");
      }
    },
  },
  computed: {
    isRUcitizenship() {
      return this.formData.citizenship === "🇷🇺";
    },
    fioWasChanged() {
      return this.formData.fioWasChanged && this.formData.fioWasChanged === "1";
    },
    lastNameInvalid() {
      return (
        this.formData.lastName &&
        !CYRILLIC_FIO_VALIDATOR(this.formData.lastName)
      );
    },
    firstNameInvalid() {
      return (
        this.formData.firstName &&
        !CYRILLIC_FIO_VALIDATOR(this.formData.firstName)
      );
    },
    middleNameInvalid() {
      return (
        this.formData.middleName &&
        !CYRILLIC_FIO_VALIDATOR(this.formData.middleName)
      );
    },
    prevLastNameInvalid() {
      return (
        this.formData.prevLastName &&
        !CYRILLIC_FIO_VALIDATOR(this.formData.prevLastName)
      );
    },
    prevFirstNameInvalid() {
      return (
        this.formData.prevFirstName &&
        !CYRILLIC_FIO_VALIDATOR(this.formData.prevFirstName)
      );
    },
    latinLastNameInvalid() {
      return (
        this.formData.latinLastName &&
        !LATIN_FIO_VALIDATOR(this.formData.latinLastName)
      );
    },
    latinFirstNameInvalid() {
      return (
        this.formData.latinFirstName &&
        !LATIN_FIO_VALIDATOR(this.formData.latinFirstName)
      );
    },
    emailInvalid() {
      return this.formData.email && !EMAIL_VALIDATOR(this.formData.email);
    },
    bdateInvalid() {
      return this.formData.bdate && new Date(this.formData.bdate) > new Date();
    },
    passportSerialInvalid() {
      return this.formData.passportSerial.length != MAX_PASSPORT_SERIAL_LENGTH;
    },
    passportNumberInvalid() {
      return (
        this.isRUcitizenship &&
        this.formData.passportNumber.length != MAX_RU_PASSPORT_NUMBER_LENGTH
      );
    },
    isFormValid() {
      return (
        !this.lastNameInvalid &&
        !this.firstNameInvalid &&
        !this.middleNameInvalid &&
        !this.prevLastNameInvalid &&
        !this.prevFirstNameInvalid &&
        !this.latinLastNameInvalid &&
        !this.latinFirstNameInvalid &&
        !this.emailInvalid &&
        !this.bdateInvalid &&
        !this.passportSerialInvalid &&
        !this.passportNumberInvalid
      );
    },
  },
};
</script>

<style scoped>
.hint {
  margin: 0;
}

.form__group {
  display: flex;
  flex-direction: column;
  margin: 4px 0;
}

.form__item {
  margin: 8px 0;
}

.form-item {
  display: flex;
  flex-direction: column;
}

.form-item__label {
  margin-bottom: 4px;
}

.form-item__type_radio {
  flex-direction: row;
}

.form-item__type_radio .form-item__control {
  margin-right: 4px;
  order: 0;
}

.form-item__type_radio .form-item__label {
  order: 1;
}

.form-item__control_invalid {
  border-color: tomato;
}
</style>
