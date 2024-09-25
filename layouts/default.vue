<template>
  <div class="flex flex-col min-h-screen bg-[#fafafe]">
    <Header class="fixed top-0 left-0 w-full z-10" />
    <main class="flex-grow pt-16">
      <slot/>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import Header from "../components/header";
import Footer from "../components/footer";
</script>
