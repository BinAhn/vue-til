<template>
  <div>
    <form @submit.prevent="submitForm">
      <div>
        <label for="userName">ID: </label>
        <input type="text" id="userName" v-model="username" />
      </div>
      <div>
        <label for="password">PassWord: </label>
        <input type="text" id="password" v-model="password" />
      </div>
      <div>
        <label for="nickName">NickName: </label>
        <input type="text" id="nickName" v-model="nickname" />
      </div>
      <button :disabled="!isUserNameValid || !password" type="submit">
        회원가입
      </button>
      <p>{{ logMessage }}</p>
    </form>
  </div>
</template>

<script>
import { registerUser } from '@/api/index';
import { validateEmail } from '@/utils/validation';

export default {
  data() {
    return {
      username: '',
      password: '',
      nickname: '',
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
      const userData = {
        username: this.username,
        password: this.password,
        nickName: this.nickname,
      };
      const { data } = await registerUser(userData);
      console.log(data.username);
      this.logMessage = `${data.username}님이 가입되었습니다.`;
      this.initForm();
    },
    initForm() {
      (this.username = ''), (this.password = ''), (this.nickname = '');
    },
  },
};
</script>

<style></style>
