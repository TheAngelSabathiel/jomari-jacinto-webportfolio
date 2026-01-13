<template>
<h3 class="text-center py-5"><span class="first-text">Get In</span> <span class="second-text">Touch</span></h3>
				<div class="container-fluid">
					<div class="row">
						<div class="col-12 col-md text-center mt-3 mb-5 mx-0 mx-lg-3 iframe-wrapper" >
							<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3862.002904804206!2d120.99632667549707!3d14.541826478512116!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3397c95cb45f8b85%3A0x822e3130ab2f47ed!2s2644%20P.%20Villanueva%2C%20Pasay%20City%2C%201300%20Metro%20Manila!5e0!3m2!1sen!2sph!4v1759309876320!5m2!1sen!2sph" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
						</div>
						<div class="col-12 col-lg mb-5 mx-0 mx-lg-3">
							<form @submit.prevent="submitForm">
								<div class="mb-3" id="name">
									<label for="name" class="form-label">Name</label>
									<input
									v-model="name" 
									type="name"
									class="form-control bg-white"
									id="name"
									placeholder="First Name M.I. Last Name"
									required
									/>
								</div>
								<div class="mb-3" id="email">
									<label for="email" class="form-label">Email Address</label>
									<input
									v-model="email" 
									type="email"
									class="form-control bg-white"
									id="email"
									placeholder="example@email.com"
									required
									/>
								</div>
								<div class="mb-3" id="message">
									<label for="message" class="form-label">Message</label>
									<textarea v-model="message" class="form-control bg-white" placeholder="Enter your message here" id="message" rows="6" required></textarea>
								</div>
								<div class="container-fluid">
									<div class="row">
										<div class="col-2 col-lg-1 contact-icon"><a href="https://github.com/TheAngelSabathiel"><img src="/images/github.png" class="img-fluid" alt="GitHub icon"></a></div>
										<div class="col-2 col-lg-1 contact-icon"><a href="https://https://www.linkedin.com/in/jomarijacinto/"><img src="/images/linkedin.png" class="img-fluid" alt="LinkedIn icon"></a></div>
										<div class="col-2 col-lg-1"><a href="https://gitlab.com/jomari.angel.jacinto-group/jomari.angel.jacinto-project"><img src="/images/gitlab.png" class="img-fluid" alt="GitLab icon"></a></div>
										<div class="col text-end"><button type="button" class="mb-3 ms-auto" id="form-button">Submit</button></div>
										<div class="d-flex justify-content-end mt-2">
                                			<div ref="recaptchaContainer"></div>
                            			</div>
									</div>
								</div>           
							</form>
						</div>
					</div>
				</div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
import { Notyf } from "notyf";
import 'notyf/notyf.min.css';
const WEB3FORMS_ACCESS_KEY = "2506f46f-8977-412f-ae17-26810779e3cd";
const name = ref("")
const email = ref("")
const message = ref("")
const isLoading = ref(false);

const notyf = new Notyf();

//configurations needed for re-captcha
const SITE_KEY = "6Le_VkksAAAAAJDWNAdYU0gQG0cOflHFho0_BTyN";
const recaptchaContainer = ref(null);
const recaptchaWidgetId= ref(null);
const recaptchaToken = ref("");


// Callback called by reCAPTCHA when successful
function onRecaptchaSuccess(token) {
  recaptchaToken.value = token;
}

// Callback when expired
function onRecaptchaExpired() {
  recaptchaToken.value = '';
}

// Function to render the reCAPTCHA widget
function renderRecaptcha() {
  if (!window.grecaptcha) {
    console.error('reCAPTCHA not loaded');
    return;
  }

  recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
    sitekey: SITE_KEY,
    size: 'normal', // or 'compact'
    callback: onRecaptchaSuccess,
    'expired-callback': onRecaptchaExpired,
  });
}

// Function to reset reCAPTCHA 
function resetRecaptcha() {
  if (recaptchaWidgetId.value !== null) {
    window.grecaptcha.reset(recaptchaWidgetId.value);
    recaptchaToken.value = '';
  }
}



onMounted(() => {
  // This code waits for the Google reCAPTCHA library to load, then renders the reCAPTCHA widget using onMounted hook. 
  // The widget is rendered with grecaptcha.render(), which requires a sitekey. 
  // Callback functions handle success and expiration events. 
  // reCAPTCHA is reset upon form submission to clear the token.
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

const submitForm = async () => {

    if(!recaptchaToken.value) {
        notyf.error("Please verify that you are not a robot");
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

  } catch(error) {
  	console.log(error);

        isLoading.value = false;
        notyf.error("Failed to send message.");
  }
  
}
</script>