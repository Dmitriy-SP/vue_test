<script>
import { required } from "@vuelidate/validators";
import { useVuelidate } from "@vuelidate/core";

export default {
  data() {
    return {
      client: {
        secondName: "",
        firstName: "",
        surname: "",
        birthDate: "",
        phone: "",
        sex: "",
        clientGroup: "",
        currentDoctor: "",
        dontSendSMS: false,
        index: "",
        country: "",
        region: "",
        city: "",
        street: "",
        house: "",
        documentType: "",
        documentSeries: "",
        documentNumber: "",
        documentIssuedBy: "",
        documentIssuedDate: "",
      },
      clientGroups: ["VIP", "Проблемные", "ОМС"],
      doctors: ["Иванов", "Захаров", "Чернышева"],
      documentTypes: [
        "Паспорт",
        "Свидетельство о рождении",
        "Вод. удостоверение",
      ],
      hasError: false,
      errors: {
        secondName: false,
        firstName: false,
        birthDate: false,
        phone: false,
        clientGroup: false,
        city: false,
        documentType: false,
        documentIssuedDate: false,
      },
      isDone: false,
    };
  },
  computed: {
    requiredError() {
      return this.hasError ? "Обязательное поле" : "";
    },
    errorText() {
      return this.hasError ? "Заполните все обязательные поля" : "";
    },
  },
  methods: {
    send() {
      Object.keys(this.v$.client)
        .filter((key) => !key.startsWith("$"))
        .map((key) => (this.errors[key] = this.v$.client[key]["$invalid"]));
      this.hasError = !!Object.entries(this.errors)
        .flat()
        .find((item) => item === true);
      this.isDone = !this.hasError;
      return;
    },
  },
  setup: () => ({ v$: useVuelidate() }),
  validations() {
    return {
      client: {
        secondName: { required },
        firstName: { required },
        birthDate: { required },
        phone: {
          required,
          isPasswordStrong(password) {
            const regex = /^[7][0-9]{10}$/;
            return regex.test(password);
          },
        },
        clientGroup: { required },
        city: { required },
        documentType: { required },
        documentIssuedDate: { required },
      },
    };
  },
};
</script>

<template>
  <div>
    <div class="wrapper" v-bind:class="{ unactive: isDone }">
      <h1>Регистрация клиента</h1>
      <form>
        <div class="name">
          <input
            class="textInput required"
            id="secondName"
            type="text"
            v-model="client.secondName"
            placeholder="Фамилия"
          />
          <p
            class="requiredError"
            v-bind:class="{ unactive: !errors.secondName }"
          >
            Обязательное поле
          </p>
          <input
            class="textInput"
            id="firstName"
            type="text"
            v-model="client.firstName"
            placeholder="Имя"
          />
          <p
            class="requiredError"
            v-bind:class="{ unactive: !errors.firstName }"
          >
            Обязательное поле
          </p>
          <input
            class="textInput"
            id="surname"
            type="text"
            v-model="client.surname"
            placeholder="Отчество"
          />
          <p>Дата рождения:</p>
          <input
            class="textInput"
            id="birthDate"
            type="date"
            v-model="client.birthDate"
          />
          <p
            class="requiredError"
            v-bind:class="{ unactive: !errors.birthDate }"
          >
            Обязательное поле
          </p>
          <input
            class="textInput"
            id="phone"
            type="text"
            v-model="client.phone"
            placeholder="Номер телефона, начиная с 7"
          />
          <p class="requiredError" v-bind:class="{ unactive: !errors.phone }">
            Укажите телефон в формате 70000000000
          </p>
          <p class="requiredError" v-bind:class="{ unactive: !errors.phone }">
            Обязательное поле
          </p>
          <input
            class="textInput"
            id="sex"
            type="text"
            v-model="client.sex"
            placeholder="Пол"
          />
          <p>Группа клиентов:</p>
          <select id="clientGroup" v-model="client.clientGroup" multiple>
            <option v-for="clientGroup in clientGroups" :key="clientGroup">
              {{ clientGroup }}
            </option>
          </select>
          <p
            class="requiredError"
            v-bind:class="{ unactive: !errors.clientGroup }"
          >
            Обязательное поле
          </p>
          <select id="currentDoctor" v-model="client.currentDoctor">
            <option value="" selected hidden disabled>Лечащий врач</option>
            <option v-for="doctor in doctors" :key="doctor">
              {{ doctor }}
            </option>
          </select>
          <div>
            <input
              type="checkbox"
              id="dontSendSMS"
              v-model="client.dontSendSMS"
            />
            <label for="dontSendSMS">Не отправлять СМС</label>
          </div>
        </div>
        <div class="adress">
          <p>Адрес</p>
          <input
            class="textInput"
            id="index"
            type="text"
            v-model="client.index"
            placeholder="Индекс"
          />
          <input
            class="textInput"
            id="country"
            type="text"
            v-model="client.country"
            placeholder="Страна"
          />
          <input
            class="textInput"
            id="region"
            type="text"
            v-model="client.region"
            placeholder="Область"
          />
          <input
            class="textInput"
            id="city"
            type="text"
            v-model="client.city"
            placeholder="Город"
          />
          <p class="requiredError" v-bind:class="{ unactive: !errors.city }">
            Обязательное поле
          </p>
          <input
            class="textInput"
            id="street"
            type="text"
            v-model="client.street"
            placeholder="Улица"
          />
          <input
            class="textInput"
            id="house"
            type="text"
            v-model="client.house"
            placeholder="Дом"
          />
        </div>
        <div class="document">
          <p>Паспорт</p>
          <select id="documentType" v-model="client.documentType">
            <option value="" selected hidden disabled>Тип документа</option>
            <option v-for="documentType in documentTypes" :key="documentType">
              {{ documentType }}
            </option>
          </select>
          <p
            class="requiredError"
            v-bind:class="{ unactive: !errors.documentType }"
          >
            Обязательное поле
          </p>
          <input
            class="textInput"
            id="documentSeries"
            type="text"
            v-model="client.documentSeries"
            placeholder="Серия"
          />
          <input
            class="textInput"
            id="documentNumber"
            type="text"
            v-model="client.documentNumber"
            placeholder="Номер"
          />
          <input
            class="textInput"
            id="documentIssuedBy"
            type="text"
            v-model="client.documentIssuedBy"
            placeholder="Кем выдан"
          />
          <p>Дата выдачи:</p>
          <input
            class="textInput"
            id="documentIssuedDate"
            type="date"
            v-model="client.documentIssuedDate"
            placeholder="Дата выдачи"
          />
          <p
            class="requiredError"
            v-bind:class="{ unactive: !errors.documentIssuedDate }"
          >
            Обязательное поле
          </p>
        </div>
      </form>
      <p class="error">{{ errorText }}</p>
      <button v-on:click="send()">Зарегестрировать</button>
    </div>
    <div class="succesSend" v-bind:class="{ unactive: !isDone }">
      <h1>Регистрация успешно завершена</h1>
    </div>
  </div>
</template>

<style scoped>
::placeholder {
  color: #fd850a;
}
.wrapper select {
  background: #1f1f1f;
  color: #fd850a;
  font-size: 16px;
  width: 85%;
  margin-top: 20px;
}
.error {
  color: #d03939;
  margin-bottom: 15px;
}

.wrapper form {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  margin: auto;
}

.wrapper form div {
  width: 300px;
}
.wrapper {
  width: 75vw;
  border-radius: 50px;
  padding-top: 50px;
  margin-top: 20px;
  margin-bottom: 20px;
  padding-bottom: 20px;
  background: #1f1f1f;
  text-align: center;
  color: #fd850a;
}

.unactive {
  display: none;
}

.succesSend {
  width: 75vw;
  border-radius: 50px;
  padding-top: 50px;
  margin-top: 20px;
  margin-bottom: 20px;
  padding-bottom: 50px;
  background: #1f1f1f;
  text-align: center;
  color: #fd850a;
}

.wrapper h1 {
  margin-bottom: 20px;
}

.wrapper p.error {
  font-size: 20px;
  margin-top: 20px;
}
.wrapper input {
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fd850a;
  font-size: 16px;
  padding: 5px 8px;
  outline: none;
  margin-top: 15px;
}

.wrapper input.textInput {
  width: 80%;
}

.wrapper input:focus {
  border-bottom-color: #fd850a;
}

.wrapper button {
  background: #5b2915;
  color: #f7efed;
  border-radius: 10px;
  border: 2px solid #db6b3d;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
}

.wrapper button:hover:enabled {
  transition: transform 500ms ease;
  transform: scale(1.05);
}

.wrapper label {
  margin-left: 8px;
}
.wrapper button:disabled {
  background: #434343;
  cursor: not-allowed;
}

.requiredError {
  color: #d03939;
  font-size: 15px;
}

#clientGroup {
  padding: 8px;
  height: 80px;
}

p {
  margin-top: 10px;
}
</style>
