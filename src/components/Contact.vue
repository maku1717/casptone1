<template>
  <!-- start contact -->
  <div class="container my-5" id="contact">
    <!-- Title -->
    <h1 class="mb-5 py-5">Contact</h1>
    <div class="row justify-content-center gap-4 text-light">
      <!-- Email -->
      <div
        class="contact-item text-center py-4 px-3 col-10 col-md-5 col-lg-4 border border-primary shadow rounded mx-3 mb-4"
      >
        <div class="mb-2">
          <a><i class="fa-solid fa-envelope fa-3x py-1"></i></a>
        </div>
        <h5 class="py-1">Email</h5>
        <p class="mb-1">joshuakillurama1717@gmail.com</p>
        <a href="#" target="_blank">Send a message</a>
      </div>

      <!-- WhatsApp Font Awesome -->
      <div
        class="contact-item text-center py-4 px-3 col-10 col-md-5 col-lg-4 border border-primary shadow rounded mx-3 mb-4"
      >
        <div class="mb-2">
          <a><i class="fa-brands fa-facebook-messenger fa-3x py-1"></i></a>
        </div>
        <h5 class="py-1">Messenger</h5>
        <p class="mb-1">Mark Amarille</p>
        <a href="https://www.messenger.com/" target="_blank">Send a message</a>
      </div>

      <!-- WhatsApp Image Icon -->
      <div
        class="contact-item text-center py-4 px-3 col-10 col-md-5 col-lg-4 border border-primary shadow rounded mx-3 mb-4"
      >
        <div class="mb-2">
          <a><i class="fa-brands fa-whatsapp icon mb-2 fa-3x py-1"></i></a>
        </div>
        <h5 class="py-1">Mobile#</h5>
        <p class="mb-1">+69 9668120517</p>
        <a href="https://www.whatsapp.com/" target="_blank">Send a message</a>
      </div>
    </div>
  </div>

  <div
    class="d-flex justify-content-center container border border-primary shadow rounded mx-auto mb-4 col-10 col-md-5"
  >
    <form @submit.prevent="submitForm" class="container contact-form">
      <div class="mb-3">
        <label class="text-light" for="name">Name</label>
        <input
          type="text"
          class="form-control"
          placeholder="Your Full Name"
          id="name"
          required
          v-model="name"
        />
      </div>
      <div class="mb-3">
        <label class="text-light" for="email">Email</label>
        <input
          type="email"
          class="form-control"
          placeholder="Your Email"
          id="email"
          required
          v-model="email"
        />
      </div>
      <div class="mb-3">
        <label class="text-light" for="name">Message</label>
        <textarea
          class="form-control"
          rows="4"
          placeholder="Your Message"
          v-model="message"
        ></textarea>
      </div>
      <button type="submit" class="btn btn-custom" :disabled="isLoading">
        {{ isLoading ? "Sending..." : "Submit" }}
      </button>
      <!-- recaptcha checkbox -->
      <div class="d-flex justify-content-end mt-2">
        <div ref="recaptchaContainer"></div>
      </div>
    </form>
  </div>

  <!-- end of contacts -->
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";

import { Notyf } from "notyf";
import "notyf/notyf.min.css";

const notyf = new Notyf();

const WEB3FORMS_ACCESS_KEY = "d0f2162d-b71d-4b88-ab9a-771748c71093";

const subject = "New message from Portfolio Contact Form";

const name = ref("");
const email = ref("");
const message = ref("");

const isLoading = ref(false);

const submitForm = async () => {
  if (!recaptchaToken.value) {
    notyf.error("Please verify that you are not a robot.");
    return;
  }

  isLoading.value = true;

  try {
    const response = await fetch("https://api.web3forms.com/submit", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json",
      },
      body: JSON.stringify({
        access_key: WEB3FORMS_ACCESS_KEY,
        subject: subject,
        name: name.value,
        email: email.value,
        message: message.value,
      }),
    });

    const result = await response.json();

    if (result.success) {
      console.log(result);

      isLoading.value = false;
      notyf.success("Message Sent!");
    }
  } catch (error) {
    console.log(error);

    isLoading.value = false;
    notyf.error("Failed to send message");
  } finally {
    resetRecaptcha();
  }
};

const SITE_KEY = "6LdHLBIsAAAAAHjGWgP2SAgGMaoNcCcGl5Isg1FO";

const recaptchaContainer = ref(null);
const recaptchaWidgetId = ref(null);
const recaptchaToken = ref("");

function onRecaptchaSuccess(token) {
  recaptchaToken.value = token;
}

function onRecaptchaExpired() {
  recaptchaToken.value = "";
}

function renderRecaptcha() {
  if (!window.grecaptcha) {
    console.error("reCAPTCHA not loaded");
    return;
  }

  recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
    sitekey: SITE_KEY,
    size: "normal",
    callback: onRecaptchaSuccess,
    "expired-callback": onRecaptchaExpired,
  });
}

function resetRecaptcha() {
  if (recaptchaWidgetId.value !== null) {
    window.grecaptcha.reset(recaptchaWidgetId.value);
    recaptchaToken.value = "";
  }
}

onMounted(() => {
  const interval = setInterval(() => {
    if (window.grecaptcha && window.grecaptcha.render) {
      renderRecaptcha();
      clearInterval(interval);
    }
  }, 100);

  onBeforeUnmount(() => {
    clearInterval(interval);
  });
});
</script>