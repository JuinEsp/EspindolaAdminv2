<script setup>
import { ref } from "vue";
import { Head, Link, router } from "@inertiajs/vue3";
import ApplicationMark from "@/Components/ApplicationMark.vue";
import Banner from "@/Components/Banner.vue";
import Dropdown from "@/Components/Dropdown.vue";
import DropdownLink from "@/Components/DropdownLink.vue";
import NavLink from "@/Components/NavLink.vue";
import ResponsiveNavLink from "@/Components/ResponsiveNavLink.vue";

defineProps({
  title: String,
});

const showingNavigationDropdown = ref(false);

const switchToTeam = (team) => {
  router.put(
    route("current-team.update"),
    {
      team_id: team.id,
    },
    {
      preserveState: false,
    }
  );
};

const logout = () => {
  router.post(route("logout"));
};
</script>

<template>
  <div v-app>
    <Head :title="title" />

    <Banner />
    <div class="min-h-screen bg-gray-100 dark:bg-gray-900">
      <nav>
        <v-card>
          <v-layout>
            <!-- <v-system-bar color="deep-purple darken-3"></v-system-bar> -->

            <v-app-bar color="primary" prominent>
              <v-app-bar-nav-icon
                variant="text"
                @click.stop="drawer = !drawer"
              ></v-app-bar-nav-icon>

              <v-toolbar-title>My files</v-toolbar-title>

              <v-spacer></v-spacer>

              <v-btn variant="text" icon="mdi-magnify"></v-btn>

              <v-btn variant="text" icon="mdi-filter"></v-btn>

              <v-btn variant="text" icon="mdi-dots-vertical"></v-btn>
            </v-app-bar>

            <v-navigation-drawer v-model="drawer" location="bottom" temporary>
              <v-list :items="items"></v-list>
            </v-navigation-drawer>
          </v-layout>
        </v-card>
      </nav>

      <!-- Page Heading -->
      <header v-if="$slots.header" class="bg-white dark:bg-gray-800 shadow">
        <div class="max-w-7xl mx-auto py-3 px-1 sm:px-6 lg:px-8"></div>
      </header>

      <!-- Page Content -->
      <main>
        <v-row no-gutters>
          <v-col cols="2" class="py-10">
           <v-card
    class="mx-auto"
  >
    <v-list v-model:opened="open">
      <v-list-item prepend-icon="fas fa-home" title="Home" style="font-size: 13px"></v-list-item>

      <v-list-group value="Users">
        <template v-slot:activator="{ props }">
          <v-list-item
            v-bind="props"
            prepend-icon="fas fa-user"
            title="Users"
          ></v-list-item>
        </template>

        <v-list-group value="Admin">
          <template v-slot:activator="{ props }">
            <v-list-item
              v-bind="props"
              title="Admin"
            ></v-list-item>
          </template>

          <v-list-item
            v-for="([title, icon], i) in admins"
            :key="i"
            :title="title"
            :prepend-icon="icon"
            :value="title"
          ></v-list-item>
        </v-list-group>

        <v-list-group value="Actions">
          <template v-slot:activator="{ props }">
            <v-list-item
              v-bind="props"
              title="Actions"
            ></v-list-item>
          </template>

          <v-list-item
            v-for="([title, icon], i) in cruds"
            :key="i"
            :value="title"
            :title="title"
            :prepend-icon="icon"
          ></v-list-item>
        </v-list-group>
      </v-list-group>
    </v-list>
  </v-card>
          </v-col>

          <v-col cols="10">
            <slot />
          </v-col>
        </v-row>
      </main>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    drawer: false,
    group: null,
    items: [
      {
        title: "Foo",
        value: "foo",
      },
      {
        title: "Bar",
        value: "bar",
      },
      {
        title: "Fizz",
        value: "fizz",
      },
      {
        title: "Buzz",
        value: "buzz",
      },
    ],
    open: ['Users'],
    admins: [
      ['Management', 'fas fa-users-gear'],
      ['Settings', 'fas fa-gear'],
    ],
    cruds: [
      ['Create', 'fas fa-plus'],
      ['Read', 'fas fa-border-all'],
      ['Update', 'fas fa-file-pen'],
      ['Delete', 'fas fa-trash'],
    ],
  }),

  watch: {
    group() {
      this.drawer = false;
    },
  },
};
</script>

<style>
.flex-grow {
  flex-grow: 1;
}
</style>
