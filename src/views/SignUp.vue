<template>
  <div class="center-content--vertical sign-up">    
    <form @submit.prevent="signUp" class="hover-box">
      <h1 class="hover-box__header hover-box__header--center">SIGN UP</h1>

      <label-input 
        label="E-mail"
        v-model="email" 
        input-type="email" 
        id="email"      
        required="required"
        placeholder="email@example.com"
        autocomplete="username">
      </label-input>

      <label-input 
        label="Password"
        v-model="password" 
        input-type="password" 
        id="password"
        required="required"
        placeholder="••••••••••"
        autocomplete="current-password">
      </label-input>

      <div>
        <btn type="submit" :is-loading="submitting" class="btn--primary btn--full">
          Sign up
          <template v-slot:icon>
            <font-awesome-icon :icon="['fas', 'user-plus']" class="icon" />    
          </template>
        </btn>      
      </div>
    </form>

    <btn :href="signInHref" :is-loading="navigating" @click.prevent="navigateToSignIn" class="btn--neutral">
      Sign in
      <font-awesome-icon :icon="['fas', 'sign-in-alt']" class="icon" />
    </btn>
  </div>
</template>

<script>
import firebase from 'firebase';
import { mapActions } from 'vuex';
import btn from '@/components/form/btn.vue';
import labelInput from '@/components/form/label-input.vue';

export default {
  name: 'SignUp',
  components: {
    btn,
    labelInput,
  },
  data() {
    return {
      email: '',
      password: '',
      signInHref: '',
      submitting: false,
      navigating: false,
    };
  },
  computed: {

  },
  methods: {
    ...mapActions('settings', ['initSettingsForNewUser']),
    signUp() {      
      this.$data.submitting = true;
      firebase.auth().createUserWithEmailAndPassword(this.$data.email, this.$data.password)
        .then(() => {
          this.$router.replace('home');
          this.initSettingsForNewUser();
        })
        .catch((error) => {
          console.log('sign Up errro', error);
          // HANDLE SIGN UP ERROR
        });
    },
    navigateToSignIn() {
      this.$data.navigating = true;
      this.$router.push('sign-in');
    },
  },
  mounted() {
    this.$data.signInHref = this.$router.resolve({ 
      name: 'sign-in',
    }).href;    
  },
};
</script>

<style lang="scss" scoped>
  .sign-up {
    padding-bottom: 5rem;

    /deep/ .btn {
      margin-top: 2rem;
    }
  }
</style>
