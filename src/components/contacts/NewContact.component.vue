<template>
  <div class="container mt-4">
    <h2 class="mb-4">Register new contact</h2>

    <form @submit.prevent="submit" novalidate>
      <div class="mb-3">
        <label class="form-label">Name</label>
        <input v-model="contact.name" type="text" class="form-control" :class="{'is-invalid': v$.contact.name.$error}"/>
        <div class="invalid-feedback">Name is required and don´t need numerics values</div>
      </div>

      <div class="mb-3">
        <label class="form-label">Email</label>
        <input v-model="contact.email" type="text" class="form-control"
               :class="{'is-invalid': v$.contact.email.$error}"/>
        <div class="invalid-feedback">Email is required and need be a valid email</div>
      </div>

      <div class="mb-3">
        <label class="form-label">Address</label>
        <input v-model="contact.address" type="text" class="form-control"
               :class="{'is-invalid': v$.contact.address.$error}"/>
        <div class="invalid-feedback">Address is required</div>
      </div>

      <div class="mb-3">
        <label class="form-label">Phone</label>
        <input v-model="contact.phone" type="number" class="form-control"
               :class="{'is-invalid': v$.contact.phone.$error}" minlength="7" maxlength="11"/>
        <div class="invalid-feedback">Phone is required</div>
      </div>

      <div class="mb-3">
        <label class="form-label">Country</label>
        <select v-model="contact.country" class="form-select" :class="{'is-invalid': v$.contact.country.$error}">
          <option disabled selected>Select one</option>
          <option value="USA">USA</option>
          <option value="Canada">Canada</option>
          <option value="UK">UK</option>
          <option value="Australia">Australia</option>
        </select>
        <div class="invalid-feedback">You need select a country</div>
      </div>

      <div class="mb-3">
        <label class="form-label">City</label>
        <input v-model="contact.city" type="text" class="form-control" :class="{'is-invalid': v$.contact.city.$error}"/>
      </div>

      <button type="submit" class="btn btn-primary">Save</button>
    </form>
  </div>

</template>

<script>
import {reactive} from 'vue'
import useVuelidate from '@vuelidate/core'
import {required, alpha, email, minLength, maxLength} from '@vuelidate/validators'

export default {
  name: 'NewContact',
  data() {
    return {
      title: ' NewContact',
      contact: reactive({
        name: '',
        email: '',
        address: '',
        phone: '',
        country: '',
        city: '',
      }),
      v$: null
    }
  },
  components: {
    // Registro de componentes que se utilizaran.
  },
  created() {
    const rules = {
      contact: {
        name: {required, alpha},
        email: {required, email},
        address: {required},
        phone: {required, minLength: minLength(7), maxLength: maxLength(11)},
        country: {required},
        city: {},
      }
    }
    this.v$ = useVuelidate(rules, {contact: this.contact})
  },
  methods: {
    // métodos que se pueden llamar desde la plantilla o desde otras partes del componente.
    async submit() {
      const isValid = await this.v$.$validate()
      if (!isValid) {
        alert('please complete correctly the Form.')
        return
      }
      this.$emit('created', {...this.contact});
    }
  },
  computed: {
    // propiedades computadas que dependen de otras propiedades reactivas
  },
  props: {
    // propiedades que el componente puede recibir.
  },
  emits: [] // los eventos personalizados que el componente puede emitir.
}
</script>

<style></style>