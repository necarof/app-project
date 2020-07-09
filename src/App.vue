<template>
  <div class="container">
    <form class="pt-3">
      <div class="form-group">
        <label for="email">Email</label>
        <input 
          type="email" 
          id="email" 
          class="form-control"
          @blur="$v.email.$touch()"
          v-model="email"
        >
      </div>
      <pre>
        {{ $v.email }}
      </pre>
    </form>
  </div>
</template>

<script>
import { required, email } from 'vuelidate/lib/validators'

export default {
  name: 'App',
  data () {
    return {
      email: ''
    }
  },
  validations: {
    email: {
      required,
      email
    }
  }
}
</script>

<style>

</style>
