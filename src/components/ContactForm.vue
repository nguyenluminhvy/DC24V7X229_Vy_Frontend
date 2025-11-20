<script>
import * as yup from 'yup'
import { Form, Field, ErrorMessage } from 'vee-validate'

export default {
  components: {
    Form,
    Field,
    ErrorMessage,
  },
  emits: ['submit:contact', 'delete:contact'],
  props: {
    contact: { type: Object, required: true },
  },
  data() {
    const contactFormSchema = yup.object().shape({
      name: yup.string().required('Tên phải có giá trị').min(2).max(50),
      email: yup.string().email('E-mail không đúng').max(50),
      address: yup.string().max(100),
      phone: yup.string().matches(/((09|03|07|08|05)+([0-9]{8})\b)/g, 'Số điện thoại không hợp lệ'),
      gender: yup.string().required('Vui lòng chọn giới tính'),
      job: yup.string().required('Vui lòng chọn nghề nghiệp'),
    })

    return {
      contactLocal: { ...this.contact },

      genders: ['Nam', 'Nữ', 'Khác'],
      jobs: ['Sinh viên', 'Kỹ sư', 'Giáo viên', 'Nhân viên văn phòng', 'Khác'],

      contactFormSchema,
    }
  },
  methods: {
    submitContact() {
      this.$emit('submit:contact', this.contactLocal)
    },
    deleteContact() {
      this.$emit('delete:contact', this.contactLocal.id)
    },
    Cancel() {
      if (window.confirm('You have unsaved changes! Do you want to leave?')) {
        this.$router.push({ name: 'contactbook' })
      }
    },
  },
}
</script>

<template>
  <Form @submit="submitContact" :validation-schema="contactFormSchema">
    <div class="form-group">
      <label for="name">Tên</label>
      <Field name="name" type="text" class="form-control" v-model="contactLocal.name" />
      <ErrorMessage name="name" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="email">E-mail</label>
      <Field name="email" type="email" class="form-control" v-model="contactLocal.email" />
      <ErrorMessage name="email" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="address">Địa chỉ</label>
      <Field name="address" type="text" class="form-control" v-model="contactLocal.address" />
      <ErrorMessage name="address" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="phone">Điện thoại</label>
      <Field name="phone" type="tel" class="form-control" v-model="contactLocal.phone" />
      <ErrorMessage name="phone" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="gender">Giới tính</label>
      <Field as="select" name="gender" class="form-control" v-model="contactLocal.gender">
        <option disabled value="">-- Chọn giới tính --</option>
        <option v-for="g in genders" :key="g" :value="g">{{ g }}</option>
      </Field>
      <ErrorMessage name="gender" class="error-feedback" />
    </div>

    <div class="form-group">
      <label for="job">Nghề nghiệp</label>
      <Field as="select" name="job" class="form-control" v-model="contactLocal.job">
        <option disabled value="">-- Chọn nghề nghiệp --</option>
        <option v-for="j in jobs" :key="j" :value="j">{{ j }}</option>
      </Field>
      <ErrorMessage name="job" class="error-feedback" />
    </div>

    <div class="form-group form-check">
      <input type="checkbox" class="form-check-input" v-model="contactLocal.favorite" />
      <label class="form-check-label"><strong>Liên hệ yêu thích</strong></label>
    </div>

    <div class="form-group">
      <button class="btn btn-primary">Lưu</button>
      <button
        v-if="contactLocal._id"
        type="button"
        class="ml-2 btn btn-danger"
        @click="deleteContact"
      >
        Xóa
      </button>
      <button type="button" class="ml-2 btn btn-danger" @click="Cancel">Thoát</button>
    </div>
  </Form>
</template>

<style scoped>
@import '@/assets/form.css';
</style>
