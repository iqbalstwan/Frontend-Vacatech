<template>
  <b-container>
    <b-row>
      <b-col class="text-left">
        <h3>Halo, Vacafriends</h3>
        <p>
          Solusi tepat untuk mencari talent terbaik.
        </p>
      </b-col>
    </b-row>
    <b-row class="login-form">
      <b-col>
        <b-alert show variant="danger" v-show="isError">{{ error }}</b-alert>
        <b-alert show variant="success" v-show="isSuccess"
          >Activation link has been sent to your email.</b-alert
        >
        <b-form @submit.prevent="onSubmit" v-show="!isSuccess">
          <b-row class="component-form">
            <b-col class="text-left">
              <label for="name">Nama</label>
              <b-input
                type="text"
                id="name"
                v-model="form.user_name"
                placeholder="Masukkan nama panjang"
                required
              ></b-input>
            </b-col>
          </b-row>
          <b-row class="component-form">
            <b-col class="text-left">
              <label for="email">Email</label>
              <b-input
                type="email"
                id="email"
                v-model="form.user_email"
                placeholder="Masukkan alamat email"
                required
              ></b-input>
            </b-col>
          </b-row>
          <b-row class="component-form">
            <b-col class="text-left">
              <label for="company">Perusahaan</label>
              <b-input
                type="text"
                id="company"
                v-model="form.company_name"
                placeholder="masukkan nama perusahaan"
                required
              ></b-input>
            </b-col>
          </b-row>
          <b-row class="component-form">
            <b-col class="text-left">
              <label for="jabatan">Jabatan</label>
              <b-input
                type="text"
                id="jabatan"
                v-model="form.company_depart"
                placeholder="Posisi di perusahaan Anda"
                required
              ></b-input>
            </b-col>
          </b-row>
          <b-row class="component-form">
            <b-col class="text-left">
              <label for="handphone">No handphone</label>
              <b-input
                type="text"
                id="handphone"
                v-model="form.user_phone"
                placeholder="Masukkan no. handphone"
                required
              ></b-input>
            </b-col>
          </b-row>
          <b-row class="component-form">
            <b-col class="text-left">
              <label for="password">Kata sandi</label>
              <b-input
                type="password"
                id="password"
                v-model="form.user_password"
                placeholder="Masukkan kata sandi"
                required
              ></b-input>
            </b-col>
          </b-row>
          <b-row class="component-form">
            <b-col class="text-left">
              <label for="confirmpassword">Konfirmasi kata sandi</label>
              <b-input
                type="password"
                id="confirmpassword"
                v-model="form.confirm_password"
                placeholder="Masukkan konfirmasi kata sandi"
                required
              ></b-input>
            </b-col>
          </b-row>
          <b-row class="component-form">
            <b-col>
              <b-button
                type="submit"
                block
                style="background-color: #FBB017; border-color: transparent;"
                >Daftar</b-button
              >
            </b-col>
          </b-row>
        </b-form>
      </b-col>
    </b-row>
    <b-row>
      <b-col class="text-center" v-show="!isSuccess">
        <p>
          Anda sudah punya akun?
          <router-link to="/login">
            <span style="color: #FBB017">Masuk disini</span>
          </router-link>
        </p>
      </b-col>
    </b-row>
  </b-container>
</template>

<style scoped>
p {
  margin: 0;
}

.login-form {
  padding: 3vh 0vw;
}

.component-form {
  padding: 1.8vh 0vw;
}
</style>

<script>
import { mapActions } from 'vuex'

export default {
  name: 'FormLogin',
  data() {
    return {
      form: {},
      isError: false,
      isSuccess: false,
      error: ''
    }
  },
  methods: {
    ...mapActions(['register', 'activateEmail']),
    onSubmit() {
      const payload = {
        form: this.form,
        role: 'recruiter'
      }
      this.register(payload)
        .then(result => {
          const activate = {
            user_email: this.form.user_email
          }
          this.activateEmail(activate)
            .then(result => {
              this.isError = false
              this.isSuccess = true
            })
            .catch(error => {
              this.isError = true
              this.error = error.data.msg
            })
        })
        .catch(error => {
          this.isError = true
          this.error = error.data.msg
        })
    }
  }
}
</script>
