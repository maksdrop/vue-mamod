<template>
  <div class="registry">
    <div class="registry__container">
      <h1 class="registry__title">Регистрация</h1>
      <div class="registry__form form">
        <div class="form__wrapper">
          <h3 class="form__title">Заполните Ваши данные</h3>
          <div class="form__container">
            <div class="form__input-name">
              <input
                :class="[this.errorFields.name ? 'error' : '','form__input']"
                @input="nameValidator($event)"
                type="text"
                placeholder="Имя"
              >
              <span v-if="this.errorFields.name" class="error__img"></span>
              <span v-if="this.errorFields.name" class="error__message">
                {{ this.dataForm.username.length === 0 
                ?
                'Поле не может быть пустым'
                :
                'Слишком короткое имя'
                }}
              </span>
            </div>
            <div class="form__input-email">
              <input
                :class="[this.errorFields.email ? 'error' : '','form__input']"
                @input="emailValidator($event)"
                type="text"
                placeholder="Email"
              >
              <span v-if="this.errorFields.email" class="error__img"></span>
              <span v-if="this.errorFields.email" class="error__message">
                Некорректный e-mail
              </span>
            </div>
            <MySelect 
              class="form__select"
              :model-value="post"
              :options="posts"
              :errorRole="errorFields.role"
              @updatePost="updateData"
            />
            <div class="form__input-password">
              <input
                :class="[this.errorFields.password ? 'error' : '','form__input']"
                @input="passwordValidator($event)"
                :type="passwordType"
                v-model="password"
                placeholder="Пароль"
              >
              <span v-if="this.errorFields.password" class="error__img error__img-password"></span>
              <span v-if="this.errorFields.password" class="error__message">
                Слишком короткий пароль
              </span>
              <img
                class="form__input-show"
                @click="switchVisibility" 
                v-if="passwordType === 'password'" 
                src="../assets/img/showed.svg" 
                alt="Показать"
              >
              <img
                class="form__input-hiden"
                @click="switchVisibility" 
                v-else 
                src="../assets/img/hided.svg" 
                alt="Скрыть"
              >
            </div>
            <div class="form__input-repeat">
              <input
                :class="[this.errorFields.passRepeat ? 'error' : '','form__input']"
                @input="passRepeatValidator($event)"
                :type="passwordTypeRepeat"
                v-model="passRepeat"
                placeholder="Повторите пароль"
              >
              <span v-if="this.errorFields.passRepeat" class="error__img error__img-password"></span>
              <span v-if="this.errorFields.passRepeat" class="error__message">
                Несовпадение пароля
              </span>
              <img
                class="form__input-show"
                @click="switchVisibilityRepeat" 
                v-if="passwordType === 'password'" 
                src="../assets/img/showed.svg" 
                alt="Показать"
              >
              <img
                class="form__input-hiden"
                @click="switchVisibilityRepeat" 
                v-else 
                src="../assets/img/hided.svg" 
                alt="Скрыть"
              >
            </div>
          </div>
        </div>
        <div class="form__footer">
          <div class="form__switch">
            <div class="form__switch-container">
              <label class="form__switch-label">
                <input type="checkbox" v-model="defaultSwitch">
                <span class="slider"></span>
              </label>
              <p class="form__switch-question">Хотите чтобы Ваш профиль видели другие участники платформы?</p>
            </div>
            <p class="form__switch-text">Включает профиль для просмотра другими пользователями по ссылке</p>
          </div>
          <div class="form__checkbox">
            <label :class="[checked ? 'form__checkbox-label--color' : '', 'form__checkbox-label']">
              <input type="checkbox" v-model="checked">
              <img v-if="checked" src="../assets/img/check.svg" alt="Галочка">
            </label>
            <p class="form__checkbox-text">Регестрируясь, Вы соглашаетесь с <span>политикой конфиденциальности</span> и обработкой <span>персональных данных</span></p>
          </div>
          <button class="form__button" @click="formSubmit" type="button">Зарегистрироваться</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import MySelect from '../components/UI/MySelect.vue';
import axios from 'axios';

export default {
  name: 'TheForm',

  components: {
    MySelect
  },
  props: {
    msg: String,
    
  },
  data() {
    return {
      passwordType: "password",
      passwordTypeRepeat: "password",
      passRepeat: "",
      password: "",
      defaultSwitch: true,
      checked: true,
      errorForm: false,
      post: "Должность",
      posts: [
        { number: "1", name: "фронтенд-разработчик" },
        { number: "2", name: "бэкенд-разработчик" },
        { number: "3", name: "тестировщик" },
        { number: "4", name: "дата-аналитик" },
      ],
      errorFields: {
        name: false,
        email: false,
        role: false,
        password: false,
        passRepeat: false
      },
      dataForm: {
        public: true,
        username: "",
        role: "",
        email: "",
        password: "",
        password_repeat: "",
      }
    }
  },
  methods: {
    updateData(data) {
      this.post = data
    },
    switchVisibility() {
      this.passwordType = this.passwordType === 'password' ? 'text' : 'password';
    },
    switchVisibilityRepeat() {
      this.passwordTypeRepeat = this.passwordTypeRepeat === 'password' ? 'text' : 'password';
    },
    nameValidator(event) {
      if (event.target.value.length <= 3) {
        this.errorFields.name = true
      } else {
        this.errorFields.name = false
      }
      this.dataForm.username = event.target.value;
    },
    emailValidator(event) {
      // eslint-disable-next-line
      const varif = /^(([^<>()[\]\.,;:\s@\"]+(\.[^<>()[\]\.,;:\s@\"]+)*)|(\".+\"))@(([^<>()[\]\.,;:\s@\"]+\.)+[^<>()[\]\.,;:\s@\"]{2,})$/i
        .test(event.target.value)
      if (!varif) {
        this.errorFields.email = true
      } else {
        this.errorFields.email = false
      }
      this.dataForm.email = event.target.value;
    },
    passwordValidator(event) {
      if (event.target.value.length <= 5) {
        this.errorFields.password = true
      } else {
        this.errorFields.password = false
      }
      this.dataForm.password = event.target.value;
    },
    passRepeatValidator(event) {
      if (event.target.value != this.dataForm.password) {
        this.errorFields.passRepeat = true;
      } else {
        this.errorFields.passRepeat = false;
      }
      this.dataForm.password_repeat = event.target.value;
    },
    formValidator() {
      if (this.dataForm.username.length <= 3) {
        this.errorFields.name = true;
      }
      if (this.dataForm.email.length < 5) {
        this.errorFields.email = true;
      }
      if (this.dataForm.password.length <= 5) {
        this.errorFields.password = true;
      } else if (this.dataForm.password != this.dataForm.password_repeat) {
        this.errorFields.passRepeat = true;
      }
      if (this.post === "Должность") {
        this.errorFields.role = true;
      } else {
        this.dataForm.role = this.post;
        this.errorFields.role = false;
      }


      if (this.errorFields.name || this.errorFields.email || this.errorFields.password || this.errorFields.passRepeat || this.errorFields.role || !this.checked) {
        this.errorForm = true;
      } else {
        this.errorForm = false;
      }
    },
    async formGo() {
      try {
        const response = await axios.post('http://localhost:3000/profile', this.dataForm);
        console.log("Форма отправлена", response.data)
        this.$emit('updateRegistration', true);
      } catch (error) {
        console.log("Ошибка")
      }
    },
    formSubmit() {
      this.formValidator();
      console.log(this.errorForm)
      if (!this.errorForm) {
        console.log("11111")
        this.formGo()
      }
    }
  }
}
</script>

<style lang="scss" scoped>

.registry {
  display: flex;
  align-items: center;
  height: 100vh;
  background-color: #f5f5f5;

  &__container {
    max-width: 960px;
    margin: 0 auto;
    background-color: #ffffff;
    border-radius: 15px;
  }

  &__title {
    margin: 0;
    padding: 17px 0;
    padding-left: 31px;
    font-size: 19px;
    font-weight: 700;
    line-height: 27px;
  }
}
.form {

  &__wrapper {
    padding-left: 31px;
    padding-right: 15px;
    border-top: 1px solid #d9d9d9;
    border-bottom: 1px solid #d9d9d9;
  }

  &__container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-areas: 
    'name email'
    '. select'
    'password repeat';
    gap: 31px 14px;
    padding-top: 34px;
    padding-bottom: 30px;
  }

  &__title {
    margin: 0;
    padding-top: 17px;
    font-size: 16px;
    font-weight: 500;
    line-height: 19px;
  }

  &__select {
    grid-area: select;
  }

  &__input {
    width: 100%;
    padding: 10px;
    font-family: "Montserrat", Arial, sans-serif;
    font-size: 14px;
    line-height: 19px;
    border-radius: 11px;
    border: 1px solid #e6e6eb;

    &::placeholder {
      color: #9292a0;
    }

    &:focus {
      outline: none;
      border-color: #3586ff;
    }

    &:hover {
      border-color: #3586ff;
    }

    &-name {
      position: relative;
      grid-area: name;
    }

    &-email {
      position: relative;
      grid-area: email;
    }

    &-password {
      position: relative;
      grid-area: password;

      img {
        position: absolute;
        right: 10px;
        cursor: pointer;

        &:hover {
          opacity: 0.7;
        }
      }
    }

    &-repeat {
      position: relative;
      grid-area: repeat;

      img {
        position: absolute;
        right: 10px;
        cursor: pointer;

        &:hover {
          opacity: 0.7;
        }
      }
    }

    &-show {
      top: 15px;
    }

    &-hiden {
      top: 14px;
    }
  }

  &__footer {
    display: grid;
    grid-template-columns: 1fr 300px;
    grid-template-areas: 
    'switch .'
    'checkbox button';
    row-gap: 30px;
    padding: 23px 15px 65px 31px;
  }

  &__switch {
    grid-area: switch;

    &-container {
      display: flex;
      gap: 5px;
    }

    &-label {
      position: relative;
      display: inline-block;
      width: 40px;
      height: 20px;

      input {
        opacity: 0;
        width: 0;
        height: 0;
      }

      span {
        position: absolute;
        cursor: pointer;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: #ccc;
        -webkit-transition: 0.4s;
        transition: 0.4s;
        border-radius: 34px;

        &::before {
          position: absolute;
          content: "";
          height: 20px;
          width: 20px;
          left: -1px;
          bottom: 0;
          background-color: white;
          -webkit-transition: 0.4s;
          transition: 0.4s;
          border: 1px solid #cdced8;
          border-radius: 50%;
        }
      }
    }
    
    p {
      margin: 0;
      line-height: 19px;
    }

    &-question {
      font-size: 16px;
      font-weight: 500;
    }

    &-text {
      padding-top: 10px;
      padding-left: 44px;
      font-size: 14px;
      color: #696977;
    }
  }

  &__checkbox {
    display: flex;
    gap: 14px;
    grid-area: checkbox;

    &-label {
      position: relative;
      display: inline-flex;
      width: 19px;
      height: 17px;
      border: 2px solid #3586ff;
      border-radius: 5px;
      cursor: pointer;

      &:hover {
        opacity: 0.7;
      }

      &--color {
        background-color: #3586ff;
      }

      input {
        display: none;
      }

      img {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        margin: auto;
      }
    }

    &-text {
      width: 505px;
      margin: 0;
      font-size: 14px;
      line-height: 19px;

      span {
        color: #3586ff;
      }
    }
  }

  &__button {
    grid-area: button;
    font-size: 12px;
    line-height: 19px;
    color: #497ada;
    border: 0;
    border-radius: 8px;
    background: rgba(73, 122, 218, 0.20);
    cursor: pointer;

    &:hover {
      background: rgba(0, 85, 255, 0.2);
    }
  }
}

.error {
  color: #eb5757;
  border-color: #eb5757;

  &:focus {
    color: #000000;

    & + .error__img:after {
      content: none;
    }

    & ~ .error__message {
      display: none;
    }
  }

  & + .error__img::after {
    content: '';
    position: absolute;
    right: 8px;
    top: 12px;
    display: block;
    width: 16px;
    height: 16px;
    background-image: url('../assets/img/error.svg');
    background-repeat: no-repeat;
  }

  & + .error__img-password::after {
    right: 40px;
  }

  & ~ .error__message {
    position: absolute;
    right: 0;
    bottom: -14px;
    font-size: 10px;
    color: #eb5757;
  }
}

.form__switch-label input:checked + span {
  background-color: #2196F3;
}

.form__switch-label input:focus + span {
  box-shadow: 0 0 1px #2196F3;
}

.form__switch-label input:checked + span:before {
  -webkit-transform: translateX(20px);
  -ms-transform: translateX(20px);
  transform: translateX(20px);
}
</style>
