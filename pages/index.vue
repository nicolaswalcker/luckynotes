<script setup lang="ts">
const { name, host, header } = useAppConfig();
const { loggedIn, fetch: refreshSession, clear } = useUserSession();
const colorMode = useColorMode();
const { loading, password, login } = useLogin();

colorMode.preference = "system";

const isOpen = ref(false);

const handleAuthModal = () => {
  isOpen.value = true;
};

const handleLogin = async (pass: string) => {
  password.value = pass;

  await login();
  await refreshSession();

  isOpen.value = false;
};

useSeoMeta({
  titleTemplate: "%s | Luckynotes",
  title: () => name || "Missing name",
  description: () => header.description || "Missing description",
  ogTitle: () => `${name} | Luckynotes`,
});

defineOgImageComponent("Main", {
  title: name || "Missing name",
  description: header.description || "Missing description",
  host: host || "Missing host",
});
</script>

<template>
  <UContainer>
    <Header
      :title="name"
      :is-logged="loggedIn"
      @logout="clear"
      @login="handleAuthModal"
    />
    <Hero :title="header.title" :description="header.description" />

    <AuthModal v-model="isOpen" :loading="loading" @login="handleLogin" />
  </UContainer>
</template>