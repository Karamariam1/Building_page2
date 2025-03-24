<template>
  <MyNavbar />
  <MySidebar />
  <MainCentre />
  <MySidebarR />
</template>

<script>
import MainCentre from './components/Main.vue';
import MyNavbar from './components/MyNavbar.vue';
import MySidebar from './components/MySidebar.vue';
import MySidebarR from './components/MySidebarR.vue';

export default {
  components: {
    MyNavbar,
    MySidebar,
    MySidebarR,
    MainCentre
  }
};
</script>
