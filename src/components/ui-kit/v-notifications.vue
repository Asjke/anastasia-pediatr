<template>
  <el-popover
    popper-class="v-notifications"
    placement="bottom-end"
    transition="el-zoom-in-top"
    trigger="click"
    v-model="is_show"
  >
    <div class="v-notifications__header">
      <h3 class="v-notifications__title">Уведомления</h3>
      <v-button
        class="v-notifications__close"
        mode="icon"
        @click="is_show = false"
      >
        <v-icon name="icon_close" width="24" />
      </v-button>
    </div>
    <template v-if="notifications_list.length">
      <ul class="v-notifications__list">
        <li
          class="v-notifications__item"
          v-for="item in notifications_list"
          :key="item.id"
        >
          <div class="v-notifications__item-header">
            <span class="v-notifications__item-date">
              {{ durationDate(item.created_date) }}
            </span>
            <v-button
              class="v-notifications__item-delete"
              mode="icon"
              @click="handleDelete(item)"
            >
              <v-icon name="icon_delete" width="16" />
            </v-button>
          </div>
          <p class="v-notifications__item-text" v-html="item.text" />
        </li>
      </ul>
      <div class="v-notifications__footer">
        <button class="v-notifications__btn-clear" @click="handleDeleteAll">
          Очистить список
        </button>
      </div>
    </template>
    <p class="v-notifications__empty" v-else>Уведомлений нет</p>
    <v-button class="v-notifications__btn" slot="reference" mode="icon">
      <span
        v-if="notifications_list.length"
        class="v-notifications__btn-circle"
      />
      <v-icon name="icon_notifications" width="32" fill="#595BF5" />
    </v-button>
  </el-popover>
</template>

<script>
import { mapGetters } from "vuex";
import notifications from "~/mixins/notifications";
import dates from "~/mixins/dates";
import VButton from "~/components/ui-kit/v-button";
import VIcon from "~/components/icon/v-icon";

export default {
  name: "v-notifications",
  components: { VIcon, VButton },
  mixins: [dates, notifications],

  data: () => ({
    is_show: false,
  }),

  computed: {
    ...mapGetters({
      notifications_list: "user/notification_list",
    }),
  },

  mounted() {
    this.initNotifications();
  },

  methods: {
    async initNotifications() {
      if (this.$auth.loggedIn) {
        try {
          await this.$store.dispatch("user/getNotifications");
        } catch (e) {
          this.renderAsyncErrors(e);
        }
      }
    },

    async handleDelete(item) {
      try {
        await this.$store.dispatch("user/deleteNotification", item.id);
        if (this.notifications_list.length === 1) {
          this.is_show = false;
        }
        setTimeout(() => {
          this.$store.commit(
            "user/UPDATE_NOTIFICATION_LIST",
            this.notifications_list.filter((el) => el.id !== item.id)
          );
        }, 200);
      } catch (e) {
        this.renderAsyncErrors(e);
      }
    },

    async handleDeleteAll() {
      try {
        await this.$store.dispatch("user/deleteNotificationAll");
        this.is_show = false;
        setTimeout(() => {
          this.$store.commit("user/UPDATE_NOTIFICATION_LIST", {});
        }, 200);
      } catch (e) {
        this.renderAsyncErrors(e);
      }
    },
  },
};
</script>
