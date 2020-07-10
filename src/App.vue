<template>
  <div class="container">
    <form 
      class="pt-3"
      @submit.prevent="onSubmit"
    >
      <div class="form-group">
        <label for="email">Email</label>
        <input 
          type="email" 
          id="email" 
          class="form-control"
          :class="{'is-invalid' : $v.email.$error}"
          @blur="$v.email.$touch()"
          v-model="email"
        >
        <div class="invalid-feedback" v-if="!$v.email.required">
          Email field is required
        </div>
        <div class="invalid-feedback" v-if="!$v.email.email">
          This field shold be an email
        </div>
        <div class="invalid-feedback" v-if="!$v.email.uniqEmail">
          This email is used
        </div>
      </div>

      <div class="form-group">
        <label for="password">Password</label>
        <input 
          type="password" 
          id="password" 
          class="form-control"
          :class="{'is-invalid' : $v.password.$error}"
          @input="$v.password.$touch()"
          v-model="password"
        >
        <div class="invalid-feedback" v-if="!$v.password.minLength">
          Min length of password is {{ $v.password.$params.minLength.min }}. Now it is {{ password.length }}.
        </div>
      </div>

      <div class="form-group">
        <label for="confirm">Confirm password</label>
        <input 
          type="password" 
          id="confirm" 
          class="form-control"
          :class="{'is-invalid' : $v.confirmPassword.$error}"
          @input="$v.confirmPassword.$touch()"
          v-model="confirmPassword"
        >
        <div class="invalid-feedback" v-if="!$v.confirmPassword.sameAs">
          Password shold be match
        </div>
      </div>

      <button 
        class="btn btn-primary"
        type="submit"
        :disabled="$v.$invalid"
      >
        Submit
      </button>
    </form>
  </div>
</template>

<script>
import { required, email, minLength, sameAs } from 'vuelidate/lib/validators'

export default {
  name: 'App',
  data () {
    return {
      email: '',
      password: '',
      confirmPassword: ''
    }
  },
  methods: {
    onSubmit() {
      console.log('Email', this.email)
      console.log('Password', this.password)
    }
  },
  validations: {
    email: {
      required,
      email,
      uniqEmail: function(newEmail) {
        if (newEmail === '') return true
        return new Promise((resolve, reject) => {
          setTimeout(() => {
            const value = newEmail !== 'test@email.ru'
            resolve(value)
          }, 2000)
        })
      }
    },
    password: {
      minLength: minLength(8)
    },
    confirmPassword: {
      sameAs: sameAs('password')
    }
  }
}
</script>

<style>

</style>
