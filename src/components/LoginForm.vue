<template>
  <div>
    <div>
      <form @submit.prevent="submitForm">
        <div>
          <Label for="username">ID: </Label>
          <input id="username" v-model="username" type="text" />
        </div>
        <div>
          <Label for="password">PWD: </Label>
          <input id="password" v-model="password" type="text" />
        </div>
        <button :disabled="!isUserNameValid || !password" type="submit">
          로그인
        </button>
        <p>{{ logMessage }}</p>
      </form>
    </div>
  </div>
</template>

<script>
import { loginUser } from '@/api/index';
import { validateEmail } from '@/utils/validation';

export default {
  data() {
    return {
      username: '',
      password: '',
      logMessage: '',
    };
  },
  computed: {
    isUserNameValid() {
      return validateEmail(this.username);
    },
  },
  methods: {
    async submitForm() {
      try {
        const loginData = {
          username: this.username,
          password: this.password,
        };
        const { data } = await loginUser(loginData);
        console.log(data.user.username);
        this.logMessage = `${data.user.username}님 로그인 되었습니다.`;
        this.initForm();
      } catch (error) {
        console.log(error.response);
        this.logMessage = error.response.data;
      }
    },
    initForm() {
      (this.username = ''), (this.password = '');
    },
  },
};
</script>

<style></style>
