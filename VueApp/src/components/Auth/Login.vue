<template lang="">
  <div class="container center">
    <div class="card large">
      <div class="row">
        <h3>Login :</h3>
        <form @submit.prevent="login()" class="col s12">
          <div class="row">
            <div class="input-field col s12">
              <i class="material-icons prefix ">account_circle</i>
              <input
                v-model="LoginData.email"
                id="Email"
                type="Email"
                class="validate"
              />
              <label for="icon_prefix">Email</label>
            </div>
            <div class="input-field col s12">
              <i class="material-icons prefix ">password</i>
              <input
                v-model="LoginData.password"
                id="password"
                type="password"
                class="validate"
              />
              <label for="password">Password</label>
            </div>
          </div>
          <button
            class="btn waves-effect waves-light light-blue accent-4"
            type="submit"
            name="action"
          >
            LOGIN
            <i class="material-icons right">send</i>
          </button>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import { ref } from "vue";
import { useRouter } from "vue-router";
import Api from "../../hooks/Api.js";
import Auth from "../../hooks/checkAuth.js";
import { useStore } from "vuex";
export default {
  setup() {
    const LoginData = ref({
      email: "",
      password: "",
    });

    const store = useStore();
    const router = useRouter();

    const login = async () => {
      const response = await Api.post("auth/login", LoginData.value);
      if (response.error) {
        console.log(response.error);
      } else {
        store.state.username = response.User.name;
        Auth.addUser(response.User);
        router.push("/");
      }
    };
    return {
      LoginData,
      login,
    };
  },
};
</script>
<style lang="css" scoped>
.card {
  padding: 20px;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

/* label color */
.input-field label {
  color: #000;
}
/* label focus color */
.input-field input[type="text"]:focus + label,
.input-field input[type="password"]:focus + label,
.input-field input[type="email"]:focus + label {
  color: #0091ea;
}
/* label underline focus color */
.input-field input[type="text"]:focus,
.input-field input[type="email"]:focus,
.input-field input[type="password"]:focus {
  border-bottom: 1px solid #0091ea;
  box-shadow: 0 1px 0 0 #0091ea;
}
/* valid color */
.input-field input[type="text"].valid,
.input-field input[type="email"].valid,
.input-field input[type="password"].valid {
  border-bottom: 1px solid #0091ea;
  box-shadow: 0 1px 0 0 #0091ea;
}
/* invalid color */
.input-field input[type="text"].invalid {
  border-bottom: 1px solid #0091ea;
  box-shadow: 0 1px 0 0 #0091ea;
}
/* icon prefix focus color */
.input-field .prefix.active {
  color: #0091ea;
}
</style>
