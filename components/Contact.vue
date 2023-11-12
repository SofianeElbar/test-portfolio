<script setup>
import emailjs from "@emailjs/browser";

const props = defineProps({ form: Object });
const result = ref(false);

const sendEmail = (e) => {
  e.preventDefault();
  emailjs
    .sendForm(
      "service_p6t1w9k",
      "template_psm2c4j",
      e.target,
      "oTBt4fo_LF6pTfpvs"
    )
    .then(
      (result) => {
        console.log("SUCCESS!", result.text);
      },
      (error) => {
        console.log("FAILED...", error.text);
      }
    );
  e.target.reset();
  result.value = true;
};
</script>

<template>
  <form @submit.prevent="sendEmail" class="mt-10">
    <div class="grid gap-6 sm:grid-cols-2">
      <div class="relative z-0">
        <input
          type="text"
          name="name"
          class="peer block w-full appearance-none border-0 border-b border-black bg-transparent py-2.5 px-0 text-sm text-gray-900 focus:border-blue-600 focus:outline-none focus:ring-0"
          placeholder=" "
        />
        <label
          class="absolute top-3 -z-10 origin-[0] -translate-y-6 scale-75 transform text-base text-black duration-300 peer-placeholder-shown:translate-y-0 peer-placeholder-shown:scale-100 peer-focus:left-0 peer-focus:-translate-y-6 peer-focus:scale-75 peer-focus:text-blue-600 peer-focus:dark:text-blue-500"
          >Votre nom</label
        >
      </div>
      <div class="relative z-0">
        <input
          type="email"
          name="email"
          class="peer block w-full appearance-none border-0 border-b border-black bg-transparent py-2.5 px-0 text-sm text-gray-900 focus:border-blue-600 focus:outline-none focus:ring-0"
          placeholder=" "
          required
        />
        <label
          class="absolute top-3 -z-10 origin-[0] -translate-y-6 scale-75 transform text-base text-black duration-300 peer-placeholder-shown:translate-y-0 peer-placeholder-shown:scale-100 peer-focus:left-0 peer-focus:-translate-y-6 peer-focus:scale-75 peer-focus:text-blue-600 peer-focus:dark:text-blue-500"
          >Votre email</label
        >
      </div>
      <div class="relative z-0 col-span-2">
        <textarea
          name="message"
          rows="5"
          class="peer block w-full appearance-none border-0 border-b border-black bg-transparent py-2.5 px-0 text-sm text-gray-900 focus:border-blue-600 focus:outline-none focus:ring-0"
          placeholder=" "
          required
        ></textarea>
        <label
          class="absolute top-3 -z-10 origin-[0] -translate-y-6 scale-75 transform text-base text-black duration-300 peer-placeholder-shown:translate-y-0 peer-placeholder-shown:scale-100 peer-focus:left-0 peer-focus:-translate-y-6 peer-focus:scale-75 peer-focus:text-blue-600 peer-focus:dark:text-blue-500"
          >Votre message</label
        >
      </div>
    </div>
    <button
      type="submit"
      class="mt-5 rounded-md bg-black px-10 py-2 text-white"
    >
      Envoyer
    </button>
  </form>
  <div v-if="result">Message envoyé avec succès</div>
</template>

<style scoped>
input:-webkit-autofill,
input:-webkit-autofill:hover,
input:-webkit-autofill:focus {
  -webkit-text-fill-color: inherit;
  transition: background-color 5000s ease-in-out 0s;
  -webkit-box-shadow: 0 0 0px 1000px transparent inset;
}
</style>
