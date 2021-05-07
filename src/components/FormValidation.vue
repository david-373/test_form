<template>
  <div>
    <form class="form" @submit.prevent="submitHandler" autocomplete="off">
      <div class="container">
        <div class="fullname_container">
          <div class="form_group">
            <label for="firstname">Имя*</label>
            <input
              id="firstname"
              v-model.trim="form.firstname"
              :class="{
                invalid:
                  !$v.form.firstname.required && $v.form.firstname.$dirty,
              }"
            />
            <small
              v-show="!$v.form.firstname.required && $v.form.firstname.$dirty"
              >Поле обязательно !</small
            >
          </div>
          <div class="form_group">
            <label for="patronymic">Отчество</label>
            <input id="patronymic" v-model.trim="form.patronymic" />
          </div>
          <div class="form_group">
            <label for="lastname">Фамилия*</label>
            <input
              id="lastname"
              v-model.trim="form.lastname"
              :class="{
                invalid: !$v.form.lastname.required && $v.form.lastname.$dirty,
              }"
            />
            <small
              v-show="!$v.form.lastname.required && $v.form.lastname.$dirty"
              >Поле обязательно !</small
            >
          </div>
        </div>
        <div class="form_group">
          <label>Дата рождения*</label>
          <div style="display: flex">
            <div
              style="width: 30%"
              class="select_dropdown"
              @click="showDropdown('year')"
            >
              <div class="over_select_dropdown"></div>
              <select
                :class="[
                  !$v.form.checkedDate.year.required &&
                  $v.form.checkedDate.year.$dirty
                    ? 'invalid '
                    : '',
                  show && selectName == 'year' ? 'selected' : 'select',
                ]"
              >
                <option value="">
                  {{ form.checkedDate.year }}
                </option>
              </select>
              <transition name="slide">
                <div class="dropdown_box" v-if="show && selectName == 'year'">
                  <ul>
                    <li
                      v-for="(year, i) in date.years"
                      :key="i"
                      @click="
                        form.checkedDate.year = year;
                        form.checkedDate.month
                          ? days(
                              date.months.indexOf(form.checkedDate.month) + 1
                            )
                          : '';
                      "
                    >
                      {{ year }}
                    </li>
                  </ul>
                </div>
              </transition>
            </div>
            <div
              style="width: 45%"
              class="select_dropdown"
              @click="showDropdown('month')"
            >
              <div class="over_select_dropdown"></div>
              <select
                style="text-transform: capitalize"
                :class="[
                  !$v.form.checkedDate.month.required &&
                  $v.form.checkedDate.month.$dirty
                    ? 'invalid '
                    : '',
                  show && selectName == 'month' ? 'selected' : 'select',
                ]"
              >
                <option value="">
                  {{ form.checkedDate.month }}
                </option>
              </select>
              <transition name="slide">
                <div class="dropdown_box" v-if="show && selectName == 'month'">
                  <ul>
                    <li
                      style="text-transform: capitalize"
                      v-for="(month, i) in date.months"
                      :key="i"
                      @click="(form.checkedDate.month = month), days(i + 1)"
                    >
                      {{ month }}
                    </li>
                  </ul>
                </div>
              </transition>
            </div>
            <div
              style="width: 24%"
              class="select_dropdown"
              @click="showDropdown('day')"
            >
              <div class="over_select_dropdown"></div>
              <select
                :class="[
                  !$v.form.checkedDate.day.required &&
                  $v.form.checkedDate.day.$dirty
                    ? 'invalid '
                    : '',
                  show && selectName == 'day' ? 'selected' : 'select',
                ]"
              >
                <option value="">{{ form.checkedDate.day }}</option>
              </select>
              <transition name="slide">
                <div class="dropdown_box" v-if="show && selectName == 'day'">
                  <ul>
                    <li
                      v-for="(day, i) in date.days"
                      :key="i"
                      @click="form.checkedDate.day = day"
                    >
                      {{ day }}
                    </li>
                  </ul>
                </div>
              </transition>
            </div>
          </div>
          <small
            v-show="
              (!$v.form.checkedDate.year.required &&
                $v.form.checkedDate.year.$dirty) ||
              (!$v.form.checkedDate.month.required &&
                $v.form.checkedDate.month.$dirty) ||
              (!$v.form.checkedDate.day.required &&
                $v.form.checkedDate.day.$dirty)
            "
            >Все поля обязательны !</small
          >
        </div>
        <div class="form_group" style="margin-top: 25px">
          <label for="phone">Номер телефона*</label>
          <input
            type="number"
            id="phone"
            v-model.trim="form.phoneNumber"
            :class="{
              invalid:
                (!$v.form.phoneNumber.required && $v.form.phoneNumber.$dirty) ||
                (!$v.form.phoneNumber.minLength &&
                  $v.form.phoneNumber.$dirty) ||
                (!$v.form.phoneNumber.maxLength &&
                  $v.form.phoneNumber.$dirty) ||
                (!$v.form.phoneNumber.firstSeven && $v.form.phoneNumber.$dirty),
            }"
          />
          <small
            v-show="!$v.form.phoneNumber.required && $v.form.phoneNumber.$dirty"
            >Поле обязательно !</small
          >
          <small
            v-show="
              $v.form.phoneNumber.required &&
              (!$v.form.phoneNumber.minLength ||
                !$v.form.phoneNumber.maxLength) &&
              $v.form.phoneNumber.$dirty
            "
            >Номер некоректен !</small
          >
          <small
            v-show="
              $v.form.phoneNumber.required &&
              $v.form.phoneNumber.$dirty &&
              !$v.form.phoneNumber.firstSeven &&
              !(
                !$v.form.phoneNumber.minLength || !$v.form.phoneNumber.maxLength
              )
            "
            >Номер должен начинаться с 7
          </small>
        </div>
        <div style="margin-top: 25px">
          <label>Пол</label>
          <div>
            <input
              type="radio"
              id="male"
              value="male"
              class="checkbox"
              name="gender"
              v-model="form.gender"
            />
            <label for="male">Мужчина</label>
            <input
              type="radio"
              id="female"
              value="female"
              class="checkbox"
              name="gender"
              v-model="form.gender"
            />
            <label for="female">Женщина</label>
          </div>
        </div>

        <div class="form_group" style="margin-top: 25px">
          <label for="client_group">Группа клиентов*.</label>
          <div class="select_dropdown" @click="showDropdown('client')">
            <div class="over_select_dropdown"></div>
            <select
              id="client_group"
              :class="[
                !$v.form.checkedClients.required &&
                $v.form.checkedClients.$dirty
                  ? 'invalid'
                  : '',
                show && selectName == 'client' ? 'selected' : 'select',
              ]"
            >
              <option value="">
                {{ form.checkedClients.join(", ") }}
              </option>
            </select>
            <small
              v-show="
                !$v.form.checkedClients.required &&
                $v.form.checkedClients.$dirty
              "
              >Поле обязательно !</small
            >
          </div>
          <transition name="slide">
            <div class="dropdown_box" v-if="show && selectName == 'client'">
              <ul>
                <li v-for="(client, i) in clientGroup" :key="i">
                  <input
                    class="checkbox"
                    type="checkbox"
                    :id="i"
                    :value="client"
                    v-model="form.checkedClients"
                  />
                  <label :for="i">{{ client }}</label>
                </li>
              </ul>
            </div>
          </transition>
        </div>
        <div class="form_group" style="margin-top: 25px">
          <label for="doctor">Лечащий врач</label>
          <div class="select_dropdown" @click="showDropdown('doctor')">
            <div class="over_select_dropdown"></div>
            <select
              id="doctor"
              :class="show && selectName == 'doctor' ? 'selected' : 'select'"
            >
              <option value="">
                {{ form.checkedDoctor }}
              </option>
            </select>
            <transition name="slide">
              <div class="dropdown_box" v-if="show && selectName == 'doctor'">
                <ul>
                  <li
                    v-for="(doctor, i) in doctors"
                    :key="i"
                    @click="form.checkedDoctor = doctor"
                  >
                    {{ doctor }}
                  </li>
                </ul>
              </div>
            </transition>
          </div>
        </div>
        <div class="sms">
          <input
            type="checkbox"
            class="checkbox"
            id="message"
            v-model="form.withoutSms"
          />
          <label for="message">Не отправлять СМС</label>
        </div>
        <div class="form_conainer">
          <h3>Адрес</h3>
          <div class="form_group" style="width: 30%">
            <label for="index">Индекс</label>
            <input type="number" id="index" v-model.trim="form.adress.index" />
          </div>
          <div class="form_group" style="width: 70%">
            <label for="country">Страна</label>
            <input
              type="text"
              id="country"
              v-model.trim="form.adress.country"
            />
          </div>
          <div class="form_group" style="width: 50%">
            <label for="region">Область</label>
            <input type="text" id="region" v-model.trim="form.adress.region" />
          </div>
          <div class="form_group" style="width: 50%">
            <label for="city">Город*</label>
            <input
              type="text"
              id="city"
              v-model.trim="form.adress.city"
              :class="{
                invalid:
                  !$v.form.adress.city.required && $v.form.adress.city.$dirty,
              }"
            />
            <small
              v-show="
                !$v.form.adress.city.required && $v.form.adress.city.$dirty
              "
              >Поле обязательно !</small
            >
          </div>
          <div class="form_group" style="width: 70%">
            <label for="street">Улица</label>
            <input type="text" id="street" v-model.trim="form.adress.street" />
          </div>
          <div class="form_group" style="width: 30%">
            <label for="home">Дом</label>
            <input type="text" id="home" v-model.trim="form.adress.home" />
          </div>
        </div>

        <div class="form_conainer">
          <h3>Документ</h3>
          <div class="form_group" style="width: 100%">
            <label for="document">Тип документа*</label>
            <div class="select_dropdown" @click="showDropdown('document')">
              <div class="over_select_dropdown"></div>
              <select
                id="document"
                :class="[
                  !$v.form.document.checkedDocumen.required &&
                  $v.form.document.checkedDocumen.$dirty
                    ? 'invalid'
                    : '',
                  show && selectName == 'document' ? 'selected' : 'select',
                ]"
              >
                <option value="">
                  {{ form.document.checkedDocumen }}
                </option>
              </select>
              <small
                v-show="
                  !$v.form.document.checkedDocumen.required &&
                  $v.form.document.checkedDocumen.$dirty
                "
                >Поле обязательно !</small
              >

              <transition name="slide">
                <div
                  class="dropdown_box"
                  v-if="show && selectName == 'document'"
                >
                  <ul>
                    <li
                      v-for="(document, i) in documenType"
                      :key="i"
                      @click="form.document.checkedDocumen = document"
                    >
                      {{ document }}
                    </li>
                  </ul>
                </div>
              </transition>
            </div>
          </div>
          <div class="form_group" style="width: 50%">
            <label for="series">Серия</label>
            <input
              type="number"
              id="series"
              v-model.trim="form.document.series"
            />
          </div>
          <div class="form_group" style="width: 50%">
            <label for="number">Номер</label>
            <input
              type="number"
              id="number"
              v-model.trim="form.document.number"
            />
          </div>
          <div class="form_group" style="width: 50%">
            <label for="issued">Кем выдан</label>
            <input
              type="number"
              id="issued"
              v-model.trim="form.document.issued"
            />
          </div>
          <div class="form_group" style="width: 50%">
            <label for="date">Дата выдачи*</label>
            <input
              type="date"
              id="date"
              v-model.trim="form.document.date"
              :class="{
                invalid:
                  !$v.form.document.date.required &&
                  $v.form.document.date.$dirty,
              }"
            />
            <small
              v-show="
                !$v.form.document.date.required && $v.form.document.date.$dirty
              "
              >Поле обязательно !</small
            >
          </div>
        </div>
        <div class="submit">
          <div class="spinner" v-if="animated">
            <div class="item1"></div>
            <div class="item2"></div>
            <div class="item3"></div>
            <div class="item4"></div>
            <div class="item5"></div>
          </div>
          <input
            :disabled="animated"
            type="submit"
            value="Готово"
            :class="{ submitAnimation: animated }"
          />
        </div>
      </div>
    </form>
    <SuccessModal ref="SuccessModal">
      <template v-slot:content>
        <h3 style="margin-top: 10px">
          {{ form.firstname }}, вы успешно зарегистрировались!
        </h3>
      </template>
    </SuccessModal>
  </div>
</template>

<script>
import { required, minLength, maxLength } from "vuelidate/lib/validators";
import SuccessModal from "./SuccessModal";

export default {
  components: {
    SuccessModal,
  },
  validations: {
    form: {
      firstname: {
        required,
      },
      lastname: {
        required,
      },
      checkedDate: {
        year: { required },
        month: { required },
        day: { required },
      },
      phoneNumber: {
        required,
        firstSeven(v) {
          return v[0] == 7 ? true : false;
        },
        minLength: minLength(11),
        maxLength: maxLength(11),
      },
      checkedClients: {
        required,
      },
      adress: {
        city: {
          required,
        },
      },
      document: {
        checkedDocumen: {
          required,
        },
        date: {
          required,
        },
      },
    },
  },
  data() {
    return {
      selectName: "",
      show: false,
      animated: false,
      form: {
        firstname: "",
        patronymic: "",
        lastname: "",
        checkedDate: {
          year: "",
          month: "",
          day: "",
        },
        phoneNumber: "7",
        gender: "",
        checkedClients: [],
        checkedDoctor: "",
        withoutSms: false,
        adress: {
          index: "",
          country: "",
          region: "",
          city: "",
          street: "",
          home: "",
        },
        document: {
          checkedDocumen: "",
          series: "",
          number: "",
          issued: "",
          date: "",
        },
      },

      date: {
        years: Array.from(
          { length: new Date().getFullYear() - 1905 },
          (_, i) => i + 1905
        ).reverse(),
        months: Array.from({ length: 12 }, (e, i) => {
          return new Date(null, i + 1, null).toLocaleDateString("ru", {
            month: "long",
          });
        }),
        days: Array.from({ length: 31 }, (_, i) => i + 1),
      },
      clientGroup: ["VIP", "Проблемные", "ОМС"],
      doctors: ["Иванов", "Захаров", "Чернышева"],
      documenType: [
        "Паспорт",
        "Свидетельство о рождении",
        "Вод. удостоверение",
      ],
    };
  },
  methods: {
    showDropdown(v) {
      this.show = !this.show;
      this.selectName = v;
    },
    submitHandler: function (event) {
      if (this.$v.$invalid) {
        this.$v.$touch();
        if (
          !this.$v.form.firstname.required ||
          !this.$v.form.lastname.required ||
          this.$v.form.checkedDate.$invalid ||
          this.$v.form.phoneNumber.$invalid ||
          this.$v.form.checkedClients.$invalid
        ) {
          this.scrollToTop();
        }
        return;
      } else {
        this.animated = !this.animated;
        setTimeout(() => {
          this.animated = false;
          this.$refs.SuccessModal.openModal();
        }, 3000);
      }
    },
    scrollToTop() {
      window.scrollTo(0, 0);
    },
    resetData() {
      window.location.reload();
    },
    days(v) {
      switch (v) {
        case 1:
        case 3:
        case 5:
        case 7:
        case 8:
        case 10:
        case 12:
          this.date.days = Array.from({ length: 31 }, (_, i) => i + 1);
          break;
        case 4:
        case 6:
        case 9:
        case 11:
          this.date.days = Array.from({ length: 30 }, (_, i) => i + 1);
          this.form.checkedDate.day > this.date.days.length
            ? (this.form.checkedDate.day = "")
            : "";
          break;
        case 2:
          if (
            ((this.form.checkedDate.year
              ? this.form.checkedDate.year % 4 == 0
              : 2021 % 4 == 0) &&
              !(this.form.checkedDate.year
                ? this.form.checkedDate.year % 100 == 0
                : 2021 % 100 == 0)) ||
            (this.form.checkedDate.year
              ? this.form.checkedDate.year % 400 == 0
              : 2021 % 400 == 0)
          ) {
            this.date.days = Array.from({ length: 29 }, (_, i) => i + 1);
            this.form.checkedDate.day > this.date.days.length
              ? (this.form.checkedDate.day = "")
              : "";
          } else {
            this.date.days = Array.from({ length: 28 }, (_, i) => i + 1);
            this.form.checkedDate.day > this.date.days.length
              ? (this.form.checkedDate.day = "")
              : "";
          }
          break;
        default:
          this.days.length[0]; // invalid month
      }
    },
  },
};
</script>

<style lang="sass" scoped>
@import "../sass/variables.sass"
.slide-enter,
.slide-leave-to
  transform: scaleY(0)

form
  max-width: 600px
  padding: 60px
  background-color: $co2
  margin: 0 auto
  @keyframes press
    0%
      transform: scale(1)

    50%
      transform: scale(0.9)

    100%
      transform: scale(1)

  ::-webkit-scrollbar
    width: 5px

  ::-webkit-scrollbar-track
    border-radius: 10px
    background: #f1f1f1

  ::-webkit-scrollbar-thumb
    background: $co1
    border-radius: 10px

  .container
    background-color: #fff
    padding: 10px
    border-radius: 10px
    box-shadow: $shadow

  label
    margin-left: 1%

  .sms
    display: flex
    line-height: 30px

  .invalid
    box-shadow: #f44336 2px -2px 10px 0px, #f44336 -2px 2px 10px 0px

  small
    position: absolute
    bottom: -15px
    left: 5px
    color: #f44336

  .checkbox
    -webkit-appearance: none
    -moz-appearance: none
    background-color: $co2
    border: 1px solid $co1
    height: 20px
    padding: 0px 10px
    border-radius: 10px
    position: relative
    transition: 0.1s
    cursor: pointer
    &:checked
      background-color: $co1

    &:checked:after
      content: "✔"
      font-size: 15px
      position: absolute
      top: 0px
      left: 4px
      color: #fff

  select
    -webkit-appearance: none
    -moz-appearance: none
    background-image: linear-gradient(45deg, $co2 50%, $co1 50%), linear-gradient(135deg, $co1 50%, $co2 50%)
    background-position: calc(100% - 20px) calc(1em + 2px), calc(100% - 15px) calc(1em + 2px), 100% 0
    background-size: 5px 5px, 5px 5px
    background-repeat: no-repeat

  .selected
    background-image: linear-gradient(45deg, $co1 50%, $co2 50%), linear-gradient(135deg, $co2 50%, $co1 50%)
    background-position: calc(100% - 15px) 1em, calc(100% - 20px) 1em, 100% 0
    background-size: 5px 5px, 5px 5px
    background-repeat: no-repeat
    outline: 0
    box-shadow: $shadow

  .form_group
    flex-grow: 1
    margin: 10px 0px
    position: relative
    .select_dropdown
      position: relative
      cursor: pointer
      .over_select_dropdown
        position: absolute
        top: 0
        bottom: 0
        left: 0
        right: 0

    .dropdown_box
      position: relative
      transition: transform 0.3s ease-in-out
      margin: 0px 1%
      z-index: 1
      ul
        background-color: $co2
        padding: 8px
        position: absolute
        top: -1rem
        width: -webkit-fill-available
        border-radius: 10px
        max-height: 80px
        overflow-y: scroll
        li
          display: flex
          line-height: 30px
          input
            width: 20px
            height: 20px
            background-color: #fff
            border: 1px solid $co1
            box-shadow: none
            &:checked
              background-color: $co1

          label
            cursor: pointer

          &:hover
            background-color: #c2daf4
            label,
            input
              color: $co1
              transition: 0.1s

    input,
    select
      width: -webkit-fill-available
      height: 40px
      background-color: $co2
      border: none
      border-radius: 10px
      padding: 0 10px
      outline: none
      font-size: 17px
      margin: 1%
      &:focus
        box-shadow: $shadow

    input[type="date"]::-webkit-calendar-picker-indicator
      background: url("../assets/calendar-icon.png") center/90% no-repeat
      transition: 0.3s

    input[type="date"]::-webkit-calendar-picker-indicator:hover
      opacity: 0.6

    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button
      -webkit-appearance: none
      margin: 0

    input[type="number"]
      -moz-appearance: textfield

  .fullname_container
    display: flex
    flex-wrap: wrap-reverse

  .form_conainer
    display: flex
    flex-wrap: wrap

    h3
      width: 100%
      font-size: 25px
      margin: 0px
      margin-top: 20px

  input[type="submit"]
    height: 40px
    width: 50%
    background: $co1
    border: 0 none
    border-radius: 10px
    cursor: pointer
    font-size: 17px
    letter-spacing: 1px
    color: #fff
    margin: 20px 25%
    transition: 0.3s
    &:hover
      box-shadow: $shadow

  .submit
    position: relative
    .spinner
      position: absolute
      width: 20px
      height: 25px
      right: 40%
      top: 35%
      div
        background-color: #fff
        height: 100%
        width: 3px
        border-radius: 25%
        margin-right: 1px
        display: inline-block
        animation: delay 1.2s infinite ease-in-out

      .item2
        -webkit-animation-delay: -1.1s
        animation-delay: -1.1s

      .item3
        -webkit-animation-delay: -1s
        animation-delay: -1s

      .item4
        -webkit-animation-delay: -0.9s
        animation-delay: -0.9s

      .item5
        -webkit-animation-delay: -0.8s
        animation-delay: -0.8s

    @keyframes delay
      0%,
      40%,
      100%
        transform: scaleY(0.4)
        -webkit-transform: scaleY(0.4)

      20%
        transform: scaleY(1)
        -webkit-transform: scaleY(1)

  .submitAnimation
    animation: 0.7s press

@media screen and (max-width: 620px)
  form
    padding: 10px
    .form_group
      input
        font-size: 15px

    .submit
      .spinner
        right: 36%
</style>

