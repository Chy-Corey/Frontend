<template>
  <v-app id="inspire">
    <v-navigation-drawer model-value class="pt-4" color="grey-lighten-3" rail expand-on-hover dense>
      <v-list>
        <v-list-item title="Hongyu" subtitle="">
          <template v-slot:prepend>
            <v-avatar @click.prevent="routeToHome" class="avatar">
              <v-img src="/avatar.jpg" title="toHome"></v-img>
            </v-avatar>
          </template>
          <template v-slot:append>
            <v-btn size="small" title="E-mail" variant="text" icon="mdi-menu-down"
              @click.prevent="listShow = !listShow"></v-btn>
          </template>
        </v-list-item>
        <v-list-item v-show="listShow" title="E-mail: " subtitle="chy.yubao@gmail.com" prepend-icon="null">

        </v-list-item>
      </v-list>
      <v-divider></v-divider>
      <v-list nav>
        <v-list-item v-for="item in nav.listItem" :key="item.name" :prepend-icon="item.icon" :title="item.name"
          :subtitle="item.subName" link></v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script setup>
import navList from "@/jsData/App/data.js"
import { ref } from "vue";
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()
const nav = ref(navList)
const listShow = ref(false)
const routeToHome = function () {
  if (route.path != "/") {
    router.push("/")
  }
}

</script>

<style scoped>
.avatar {
  cursor: pointer;
}
</style>
