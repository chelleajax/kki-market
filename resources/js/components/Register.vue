<template>
    <div class="flex items-center justify-center min-h-screen p-4 bg-base-300">
        <div class="w-full max-w-md p-6 space-y-6 rounded-lg shadow-lg sm:p-8 bg-base-100">
            <h1 class="text-3xl font-bold text-center sm:text-4xl text-primary">Register</h1>
            <form @submit.prevent="handleSubmit" class="space-y-4 sm:space-y-6">
            <div class="form-control">
                <label for="email" class="label">
                <span class="label-text">Email</span>
                </label>
                <input type="email" id="email" v-model="email" placeholder="Email" class="w-full input input-bordered" required/>
            </div>
            <div class="form-control">
                <label for="username" class="label">
                <span class="label-text">Username</span>
                </label>
                <input type="text" id="username" v-model="username" placeholder="Username" class="w-full input input-bordered" required />
            </div>
            <div class="form-control">
                <label for="password" class="label">
                <span class="label-text">Password</span>
                </label>
                <input type="password" id="password" v-model="password" placeholder="Password" class="w-full input input-bordered" required />
            </div>
            <div class="form-control">
                <label for="password_confirmation" class="label">
                <span class="label-text">Confirm Password</span>
                </label>
                <input type="password" id="password_confirmation" v-model="passwordConfirmation" placeholder="Confirm Password" class="w-full input input-bordered" required />
            </div>
            <div class="text-center">
                <p class="text-sm">
                Have an account?
                <router-link to="/login" class="text-primary hover:underline">Login here</router-link>.
                </p>
            </div>
            <div class="mt-6 form-control">
                <button type="submit" class="w-full btn btn-primary">Register</button>
            </div>
            </form>
        </div>
        <Popup v-model:show="popupShow" :title="popupTitle" :message="popupMessage" :type="popupType" />
    </div>
</template>

<script>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';
import Popup from './Popup.vue';

export default {
    components: { Popup },
    setup() {
        const router = useRouter();
        const email = ref('');
        const username = ref('');
        const password = ref('');
        const passwordConfirmation = ref('');
        const popupShow = ref(false);
        const popupTitle = ref('');
        const popupMessage = ref('');
        const popupType = ref('info');

        const showPopup = (title, message, type = 'info') => {
            popupTitle.value = title;
            popupMessage.value = message;
            popupType.value = type;
            popupShow.value = true;
        };

        const handleSubmit = async () => {
            try {
                const response = await axios.post('/register', {
                    email: email.value,
                    username: username.value,
                    password: password.value,
                    password_confirmation: passwordConfirmation.value,
                });
                showPopup('Success', 'Registration successful! Please log in.', 'success');
                router.push('/login');
            } catch (error) {
                showPopup('Error', error.response?.data?.error || 'Failed to register. Please try again.', 'error');
            }
        };

        return {
            email,
            username,
            password,
            passwordConfirmation,
            popupShow,
            popupTitle,
            popupMessage,
            popupType,
            handleSubmit
        };
    },
};
</script>