<template>
  <div id="app">
    <form v-if="!isSubmited" class="form-wrapper" @submit.prevent="submitHandler">

      <section id="client-info" class="form-section">
        <h3>Клиент: </h3>
        <div class="form-field" :class="{error: $v.lastName.$error}">
          <label for="last-name">Фамилия *</label>
          <input
            type="text" 
            name="last-name"
            class="input-required"
            v-model.trim="$v.lastName.$model" 
            :class="{invalid: $v.lastName.$dirty && !$v.lastName.required}">
          <div class="error-content" :class="{error: $v.lastName.$error}">
            <span v-if="$v.lastName.$dirty && !$v.lastName.required">Пожалуйста, введите фамилию</span>
          </div>
        </div>
        <div class="form-field" :class="{error: $v.name.$error}">
          <label for="name">Имя *</label>
          <input 
            type="text" 
            name="name"
            class="input-required"
            v-model.trim="$v.name.$model" 
            :class="{invalid: $v.name.$dirty && !$v.name.required}">
          <div class="error-content" :class="{error: $v.name.$error}">
            <span v-if="$v.name.$dirty && !$v.name.required">Пожалуйста, введите имя</span>
          </div>
        </div>
        <div class="form-field">
          <label for="sec-name">Отчество</label>
          <input 
            type="text" 
            name="sec-name"
            v-model.trim="secName" >
        </div>
        <div class="form-field" :class="{error: $v.birthDate.$error}">
          <label for="birth-date">Дата рождения *</label>
          <input 
            :max="maxDate()"
            type="date" 
            name="birth-date"
            v-model.trim="$v.birthDate.$model" 
            class="input-required"
            :class="{invalid: $v.birthDate.$dirty && !$v.birthDate.required}">
          <div class="error-content" :class="{error: $v.birthDate.$error}">
            <span v-if="$v.birthDate.$dirty && !$v.birthDate.required">Пожалуйста, введите дату рождения</span>
          </div>
        </div>
        <div class="form-field" :class="{error: $v.phoneNum.$error}">
          <label for="phone">Номер телефона *</label>
          <input 
            type="tel" 
            id="phone" 
            name="phone" 
            pattern="\+7\s?[0-9]{9}"
            placeholder="+7 (900) 123-45-67"
            minlength="11"
            maxlength="11"
            v-model.trim="$v.phoneNum.$model">
            <div class="error-content" :class="{error: $v.phoneNum.$error}">
              <span v-if="$v.phoneNum.$dirty && !$v.phoneNum.required">Пожалуйста, введите номер телефона</span>
              <span v-if="$v.phoneNum.$dirty && !$v.phoneNum.isPhone && $v.phoneNum.required">Пожалуйста, введите номер в формате +7 (900) 123-45-67</span>
            </div>
        </div>
        <div class="form-field">
          <label for="gender">Пол </label>
          <select v-model="selectedGender" name='gender' id="gender">
            <option disabled selected value> -- Выберите пол -- </option>
            <option :key="gender" v-for="gender in gender" :value="gender">{{gender}}</option>
          </select>
        </div>
        <div class="form-field">
          <label for="client-group">Группа </label>
          <select v-model="selectedClientGroup" multiple size=3 name='client-group' id="clientGroup">
            <option :key="value" v-for="value in clientGroup" :value="value">{{value}}</option>
          </select>
        </div>
        <div class="form-field">
          <label for="doctor">Лечащий врач </label>
          <select v-model="selectedDoctor" name='doctor' id="doctor">
            <option disabled selected value> -- Выберите лечащего врача -- </option>
            <option :key="doc" v-for="doc in doctor" :value="doc">{{doc}}</option>
          </select>
        </div>
        <input 
            style="margin-right: 20px;"
            name="mess-check" 
            type="checkbox" 
            v-model="dontSendMessages">
        <label for="mess-check">Не отправлять СМС</label>
      </section>

      <hr>
      
      <section id="address" class="form-section">
        <h3>Адрес: </h3>
        <div class="form-field">
          <label for="index">Индекс</label>
          <input 
            type="number" 
            name="index"
            v-model.trim="address.index" >
        </div>
        <div class="form-field">
          <label for="country">Страна</label>
          <input
            type="text" 
            name="country"
            v-model.trim="address.country" >
        </div>
        <div class="form-field">
          <label for="obl">Область</label>
          <input
            type="text" 
            name="obl"
            v-model.trim="address.obl" >
        </div>
        <div class="form-field" :class="{error: $v.address.city.$error}">
          <label for="city">Город *</label>
          <input
            type="text" 
            name="city"
            v-model.trim="$v.address.city.$model" 
            class="input-required"
            :class="{invalid: $v.address.city.$dirty && !$v.address.city.required}">
          <div class="error-content" :class="{error: $v.address.city.$error}">
            <span v-if="$v.address.city.$dirty && !$v.address.city.required">Пожалуйста, введите имя</span>
          </div>
        </div>
        <div class="form-field">
          <label for="street">Улица</label>
          <input
            type="text" 
            name="street"
            v-model.trim="address.street" >
        </div>
        <div class="form-field">
          <label for="house-num">Дом</label>
          <input
            type="number" 
            name="house-num"
            v-model.trim="address.number" >
        </div>
      </section>

      <hr>

      <section id="passport" class="form-section">
        <h3>Паспортные данные:</h3>
        <div class="form-field" :class="{error: $v.passport.selDocType.$error}">
          <label for="doc-type">Тип документа *</label>
          <select v-model="passport.selDocType" name='doc-type' id="docType">
            <option disabled selected> -- Выберите тип -- </option>
            <option :key="type" v-for="type in passport.docType" :value="type">{{type}}</option>
          </select>
          <div class="error-content" :class="{error: $v.passport.selDocType.$error}">
            <span v-if="$v.passport.selDocType.$dirty && !$v.passport.selDocType.required">Пожалуйста, выберите тип документа</span>
          </div>
        </div>
        <div class="passport-data">
          <div class="form-field" id="passport-data-batch">
            <label for="batch">Серия</label>
            <input 
              type="text" 
              name="batch"
              v-model.trim="passport.batch" >
          </div>
          <div class="form-field" id="passport-data-number">
            <label for="pass-num">Номер</label>
            <input 
              type="number" 
              name="pass-num"
              v-model.trim="passport.passNum" >
          </div>
        </div>
        <div class="form-field">
          <label for="issued-by">Кем выдан</label>
          <input 
            type="text" 
            name="issued-by"
            v-model.trim="passport.issuedBy" >
        </div>
        <div class="form-field" :class="{error: $v.passport.issueDate.$error}">
          <label for="issue-date">Дата выдачи * </label>
          <input 
            :max="maxDate()"
            type="date"
            name="issue-date" 
            v-model.trim="$v.passport.issueDate.$model" 
            :class="{invalid: $v.passport.issueDate.$dirty && !$v.passport.issueDate.required}">
          <div class="error-content" :class="{error: $v.passport.issueDate.$error}">
            <span v-if="$v.passport.issueDate.$dirty && !$v.passport.issueDate.required">Пожалуйста, введите дату выдачи</span>
          </div>
        </div>
      </section>

      <button class="send-button" type="submit">Отправить</button>
    </form>
    <transition v-if="isSubmited" name='fade' appear>
      <div class="submited-wrapper">
        <div class="submited">
          <h3>Спасибо за регистрацию, {{name}} {{secName}} !</h3>
          <button onClick="window.location.reload();" @click="isSubmited = false">Ок</button>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import { required, numeric, maxValue, between, withParams} from 'vuelidate/lib/validators'
const isPhone = (value) => /\+7\s?[0-9]{9}/.test(value);


export default {
  data() {
    return {
      name: '',
      lastName: '',
      secName: '',
      birthDate: '',
      phoneNum: '',
      gender: ["Мужчина", "Женщина"],
      selectedGender: '',
      clientGroup: ["VIP", "Проблемные", "ОМС"],
      selectedClientGroup: [],
      doctor: ["Иванов", "Захаров", "Чернышева"],
      selectedDoctor: '',
      dontSendMessages: false,
      address: {
        index: '',
        country: '',
        city: '',
        obl: '',
        street: '',
        number: ''
      },
      passport: {
        docType: ["Паспорт" , "Свидетельство о рождении" , "Вод. удостоверение"],
        selDocType: '',
        batch: '',
        passNum: '',
        issuedBy: '',
        issueDate: ''
      },
      isSubmited: false,

    }
  },
  validations: {
    name: { required },
    lastName: { required },
    birthDate: {required},
    phoneNum: {required, isPhone},
    clientGroup: {required},
    address: {city: {required}},
    passport: {selDocType: {required}, issueDate: {required}}

  },
  methods: {
    submitHandler() {
      console.log("Guests:\n * John\n * Pete\n * Mary")
      if (this.$v.$invalid) {
        this.$v.$touch()
      }
      else {
        this.isSubmited = true
        console.info('- Имя клиента: ' + this.name+'\n'+
                      '- Фамилия: ' + this.lastName+'\n'+
                      '- Отчество: ' + this.secName+'\n'+
                      '- Дата рождения: ' + this.birthDate+'\n'+
                      '- Номер телефона: ' + this.phoneNum+'\n'+
                      '- Пол: ' + this.selectedGender+'\n'+
                      '- Группа клиентов: ' + this.selectedClientGroup+'\n'+
                      '- Лечащий врач: ' + this.selectedDoctor+'\n'+
                      '- Не отправлять смс: ' + this.dontSendMessages+'\n'+
                      '- Индекс: ' + this.address.index+'\n'+
                      '- Страна: ' + this.address.country+'\n'+
                      '- Область: ' + this.address.obl+'\n'+
                      '- Город: ' + this.address.city+'\n'+
                      '- Улица: ' + this.address.street+'\n'+
                      '- Дом: ' + this.address.number+'\n'+
                      '- Тип документа: ' + this.passport.selDocType+'\n'+
                      '- Серия: ' + this.passport.batch+'\n'+
                      '- Номер: ' + this.passport.passNum+'\n'+
                      '- Кем выдан: ' + this.passport.issuedBy+'\n'+
                      '- Дата выдачи: ' + this.passport.issueDate+'\n')
      }
    },
    status(v) {
      return {
        error: v.$error,
        dirty: v.dirty
      }
    },
    maxDate() {
      let date = new Date()
      let year = date.getFullYear()
      let day = date.getDate()
      let month = date.getMonth() + 1
      if (day < 10) 
        day = '0' + day
      if (month < 10 )
          month = '0' + month
      return date = year + '-' + month + '-' + day
    }
  }
}
</script>

<style lang="sass">
@import 'assets/media'
body
  font-size: 16px
#app
  position: relative
  padding: 30px 0
  background: #f3f3f3
  overflow: hidden

  h3
    font-size: 25px
    margin-top: 0

  .form-wrapper
    padding: 30px 40px
    margin: auto
    width: 50vw
    background: #fff
    border: 1px #777 solid
    border-radius: 6px

    .form-section
      margin: auto

      .form-field
        position: relative
        margin: 25px 0
        border: 1px #777 solid 
        border-radius: 6px

        label
          display: block
          float: left
          margin-left: 10px
          margin-top: -12px
          background: #fff
          height: 16px
          padding: 2px 5px
          overflow: hidden

        input, select
          width: 100%
          border: 0
          padding: 12px 8px
          font-size: 1em
          border-radius: 6px
          box-sizing: border-box

          &:focus
            outline: 0
            
          &.invalid
            border-color: red
            color: red

        .error-content
          border-radius: 0 0 6px 6px
          display: flex
          justify-content: center
          align-items: center

          span
            opacity: .9

          &.error
            background: #e3e3e3
            padding: 8px 0
            border-color: red
            color: red

        small
          position: absolute
          left: 105%
          right: -100%
          top: 50%
          text-align: left

      .passport-data
        display: grid
        grid-template-columns: 1fr 1fr

        #passport-data-batch,
        #passport-data-number
          margin-top: 0
          margin-bottom: 0
        
        #passport-data-batch
          margin-right: 20px 

  .submited-wrapper
    top: 0
    left: 0
    right: 0
    bottom: 0
    z-index: 98
    background: rgba(0, 0, 0, .55)
    position: fixed
    display: flex
    justify-content: center
    align-items: center

    .submited
      background: #fff
      display: inline-block
      padding: 100px
      border-radius: 10px

      button
        padding: 5px 25px
        color: #fff
        font-weight: bold
        font-size: 20px
        background: #478ade
        border: 2px #0b67db solid
        border-radius: 10px
        display: block
        margin: auto

  hr
    margin: 30px auto
    width: 50%
    height: 1px
    background: #999
    border: 1px #999 solid
    border-radius: 50%

  .send-button
    padding: 5px 15px
    color: #fff
    font-weight: bold
    font-size: 20px
    background: #478ade
    border: 2px #0b67db solid
    border-radius: 10px
    margin-top: 30px
    display: block
    margin: auto

// @media only screen (max-width: 640px)
//   #app
//     .form-wrapper
//       .form-section
//         width: 100%
</style>
