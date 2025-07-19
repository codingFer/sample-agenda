<template>
    <div class="container mt-4">
      <h4>Edit Contact</h4>
  
      <form @submit.prevent="emitEditedData" novalidate>
        <div class="mb-3">
          <label class="form-label">Name</label>
          <input v-model="copyContact.name" type="text" class="form-control" required />
        </div>
  
        <div class="mb-3">
          <label class="form-label">Email</label>
          <input v-model="copyContact.email" type="text" class="form-control" required />
        </div>
  
        <div class="mb-3">
          <label class="form-label">Address</label>
          <input v-model="copyContact.address" type="text" class="form-control" required />
        </div>
  
        <div class="mb-3">
          <label class="form-label">Country</label>
          <select v-model="copyContact.country" class="form-select" >
            <option disabled>Select one</option>
            <option value="USA">USA</option>
            <option value="Canada">Canada</option>
            <option value="UK">UK</option>
            <option value="Australia">Australia</option>
          </select>
        </div>
  
        <div class="mb-3">
          <label class="form-label">City</label>
          <input v-model="copyContact.city" type="text" class="form-control" required />
        </div>

<!--        TODO: Align this code to the center-->
        <button type="submit" class="btn btn-success">Save Changes</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: 'EditContact',
    props: {
      contact: {
        type: Object,
        required: true
      }
    },
    data() {
      return {
        copyContact: { ...this.contact }
      }
    },
    methods: {
      emitEditedData() {
        this.$emit('update', this.copyContact)
      }
    }
  }
  </script>
  