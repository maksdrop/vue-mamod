<template>
  <div class="select">
    <div
      :class="[this.errorRole && errorSel === true ? 'error' : '', 'select__button']"
      @click="showList"
    >
      <span
        :class="[this.modelValue === 'Должность' ? 'select__placeholder' : '', 'select__option']"
      >
        {{ modelValue }}
      </span>
      <img
        :class="[this.errorRole && errorSel === true ? 'select__arrow' : '']"
        src="../../assets/img/arrow.svg"
        alt="Стрелка"
      />
      <span
        v-if="this.errorRole && errorSel === true"
        class="error__img"
      ></span>
      <span v-if="this.errorRole && errorSel === true" class="error__message">
        {{ this.errorMessageSelect }}
      </span>
    </div>
    <ul class="select__options" v-if="showSelect">
      <li
        :class="[modelValue == option.name ? 'select__active' : '']"
        v-for="option in options"
        :key="option.number"
        :value="option.number"
        @click="changeOption(option.name)"
      >
        {{ option.name }}
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  name: 'MySelect',

  data() {
    return {
      showSelect: false,
      errorMessageSelect: "Поле не может быть пустым",
      errorSel: true
    }
  },
  props: {
    modelValue: {
      type: String
    },
    options: {
      type: Array,
    },
    errorRole: {
      type: Boolean
    },
  },
  methods: {
    changeOption(name) {
      this.errorSel = false
      this.showList()
      this.$emit('updatePost', name);
    },
    showList() {
      this.showSelect = !this.showSelect
    }
  },
}
</script>

<style scoped lang="scss">
.select {
  position: relative;
  width: 100%;

  &__button {
    position: relative;
    display: flex;
    justify-content: space-between;
    gap: 8px;
    padding: 10px;
    border: 1px solid #e6e6eb;
    border-radius: 11px;
    font-size: 14px;
    font-weight: 500;
    line-height: 19px;
    color: #e6e6eb;

    &:hover {
      font-weight: 400;
      border-color: #2f80ed;
      cursor: pointer;
    }

    &-form {
      padding: 8px;
      transition: border-color 500ms ease;
    }
  }

  &__option {
    color: #000000;
  }

  &__placeholder {
    font-weight: 400;
    text-transform: none;
    color: #9292A0;
  }

  &__options {
    position: absolute;
    z-index: 2;
    top: 50px;
    width: 100%;
    margin: 0;
    padding-left: 0;
    list-style-type: none;
    border-radius: 4px;
    background: #ffffff;
    box-shadow: 0px 0px 6px 0px rgba(148, 181, 225, 0.35);

    li {
      padding: 10px 8px 10px 16px;
      font-size: 14px;
      line-height: 20px;
      color: #545454;
      border-bottom: 1px solid #eaf2fd;
      transition: background-color 700ms ease;

      &:last-child {
        border-bottom: 0;
      }

      &:hover {
        background-color: #eaf2fd;
        cursor: pointer;
      }
    }
  }

  &__arrow {
    position: absolute;
    right: 12px;
    top: 20px;
  }

  &__active {
    position: relative;
    font-weight: 700;

    &::after {
      content: '';
      position: absolute;
      top: 16px;
      right: 8px;
      display: block;
      background-image: url("../../assets/img/check.svg");
      background-repeat: no-repeat;
      background-size: 12px;
      height: 16px;
      width: 16px;
    }
  }
}

.error {
  position: relative;
  color: #eb5757;
  border-color: #eb5757;

  &:focus {

    & + .error__img::after {
      content: none;
    }

    & ~ .error__message {
      display: none;
    }
  }

  &::placeholder {
    color: #eb5757;
  }

  & > .error__img {

    &::after {
      content: '';
      position: absolute;
      right: 8px;
      top: 15px;
      display: block;
      width: 16px;
      height: 16px;
      background-image: url('../../assets/img/error.svg');
      background-repeat: no-repeat;
    }
  }

  & > .error__message {
    position: absolute;
    right: 0;
    bottom: -19px;
    font-size: 10px;
    font-weight: 400;
    text-transform: none;
    color: #eb5757;
  }
}
</style>
