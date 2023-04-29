<template>
  <el-dropdown
    v-if="$auth.loggedIn"
    :class="['v-user', `v-user--${mode}`]"
    trigger="click"
    @command="commandHandler"
  >
    <button class="v-user__container">
      <span v-if="mode === 'default'" class="v-user__avatar">
        <span>{{ $auth.user.first_name[0] }}</span>
      </span>
      <span class="v-user__name">
        {{ $auth.user.first_name || "Пользователь" }}
      </span>
      <v-icon
        v-if="mode === 'landing'"
        name="icon_triangle"
        width="11"
        height="7"
      />
      <v-icon v-else name="icon_select" width="16" fill="#00395C" />
    </button>
    <el-dropdown-menu class="v-user__menu" slot="dropdown">
      <el-dropdown-item command="enter">Личный кабинет</el-dropdown-item>
      <el-dropdown-item command="logout"> Выйти </el-dropdown-item>
    </el-dropdown-menu>
  </el-dropdown>
</template>

<script>
import VIcon from "~/components/icon/v-icon";

export default {
  name: "v-user",
  components: { VIcon },

  props: {
    mode: {
      type: String,
      default: "default",
    },
  },

  methods: {
    commandHandler(command) {
      switch (command) {
        case "enter":
          if (
            !this.$route.path.includes("applications") &&
            !this.$route.path.includes("profile")
          ) {
            this.$router.push({ name: "profile" });
          }
          break;
        case "logout":
          this.$auth.logout();
          break;
      }
    },
  },
};
</script>
