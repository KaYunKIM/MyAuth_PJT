<template>
  <form class="form">
    <h1>Signup</h1>
    <v-text-field
      v-model="username"
      :error-messages="usernameErrors"
      :counter="10"
      label="Username"
      required
      @input="$v.username.$touch()"
      @blur="$v.username.$touch()"
    ></v-text-field>
    <v-text-field
      v-model="email"
      :error-messages="emailErrors"
      label="E-mail"
      required
      @input="$v.email.$touch()"
      @blur="$v.email.$touch()"
    ></v-text-field>
    <v-text-field
      v-model="password"
      :error-messages="passwordErrors"
      :counter="8"
      label="Password"
      required
      @input="$v.password.$touch()"
      @blur="$v.password.$touch()"
    ></v-text-field>
    <v-text-field
      v-model="passwordConfirm"
      :error-messages="passwordConfirmErrors"
      :counter="8"
      label="Password Confirm"
      required
      @input="$v.passwordConfirm.$touch()"
      @blur="$v.passwordConfirm.$touch()"
    ></v-text-field>
    <v-row class="mt-3">
      <v-btn 
        class="ml-3" 
        @click="clear"
      >
        clear
      </v-btn>
      <v-spacer></v-spacer>
      <v-btn
        class="mr-4"
        @click="submit"
      >
        submit
      </v-btn>
    </v-row>
  </form>
</template>


<script>
import { validationMixin } from 'vuelidate'
import { required, minLength, maxLength, email } from 'vuelidate/lib/validators'
import { mapActions } from 'vuex'

export default {
  name: 'Signup',
  mixins: [validationMixin],

  validations: {
    username: { required, maxLength: maxLength(10) },
    email: { required, email },
    password: { required, minLength: minLength(8) },
    passwordConfirm: { required, minLength: minLength(8) },
  },

  data: () => ({
    username: '',
    email: '',
    password: '',
    passwordConfirm: '',    
  }),

  computed: {
    usernameErrors () {
      const errors = []
      if (!this.$v.username.$dirty) return errors
      !this.$v.username.maxLength && errors.push('Name must be at most 10 characters long')
      !this.$v.username.required && errors.push('Username을 입력하세요')
      return errors
    },
    emailErrors () {
      const errors = []
      if (!this.$v.email.$dirty) return errors
      !this.$v.email.email && errors.push('유효한 E-mail을 입력하세요')
      !this.$v.email.required && errors.push('E-mail을 입력하세요')
      return errors
    },
    passwordErrors () {
      const errors = []
      if (!this.$v.password.$dirty) return errors
      !this.$v.password.minLength && errors.push('비밀번호는 최소 8자리를 입력하세요')
      !this.$v.password.required && errors.push('비밀번호를 입력하세요')
      return errors
    },
    passwordConfirmErrors () {
      const errors = []
      if (!this.$v.passwordConfirm.$dirty) return errors
      if (this.password !== this.passwordConfirm) errors.push('비밀번호가 일치하지 않습니다')
      !this.$v.passwordConfirm.minLength && errors.push('비밀번호는 최소 8자리를 입력하세요')
      !this.$v.passwordConfirm.required && errors.push('비밀번호를 입력하세요')
      return errors
    },
  },

  methods: {
    submit () {
      this.$v.$touch()
      const userData = {
          username: this.username,
          email: this.email,
          password1: this.password,
          password2: this.passwordConfirm,
      }
      this.signup(userData)
    },
    clear () {
      this.$v.$reset()
      this.username = ''
      this.email = ''
      this.password = ''
      this.passwordConfirm = ''
    },
    ...mapActions(['signup'])
  },
}
</script>

<style scoped>
.form {
    margin-top: 200px;
}
</style>