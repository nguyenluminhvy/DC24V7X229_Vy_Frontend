<script>
import ContactForm from '@/components/ContactForm.vue'
import ContactService from '@/services/contact.service'

export default {
  components: { ContactForm },

  data() {
    return {
      contact: {
        name: '',
        email: '',
        address: '',
        phone: '',
        favorite: false,
        gender: '',
        job: '',
      },
    }
  },

  methods: {
    async createContact(data) {
      try {
        await ContactService.create(data)
        alert('Thêm liên hệ thành công')
        this.$router.push({ name: 'contactbook' })
      } catch (error) {
        console.log(error)
      }
    },
  },
}
</script>

<template>
  <div class="page">
    <h4>Thêm Liên hệ</h4>
    <ContactForm :contact="contact" @submit:contact="createContact" />
  </div>
</template>
