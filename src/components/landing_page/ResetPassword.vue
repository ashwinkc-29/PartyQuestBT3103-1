<template>
    <div>
        <div class="container" id="container">
            <div class="form-container sign-in-container">
                <form @submit.prevent="register">
                    <h1>Reset Password</h1>
                    <div class="social-container"></div>
                    <span>Enter your email here</span>
                    <input
                        id="email"
                        type="email"
                        placeholder="Email"
                        v-model="email"
                    />
                    <br />
                    <button type="button" v-on:click="reset()">
                        Reset
                    </button>
                </form>
            </div>
            <div class="overlay-container">
                <div class="overlay">
                    <div class="overlay-panel overlay-right">
                        <img src="../../assets/PartyQuest_Logo.png" />
                    </div>
                </div>
            </div>
        </div>
        <div>
            <ClientOnly>
                <Modal v-model="showSecondModal" title="Reset Success">
                    <form novalidate>
                        <div class="form-group">
                            <label for="formField1"
                                >Reset Password Done! <br />
                                You will receive a reset password email from us
                                shortly.</label
                            >
                        </div>
                        <div class="row modal-footer">
                            <div class="col-sm-12">
                                <div class="float-right">
                                    <button
                                        class="btn btn-primary"
                                        type="button"
                                        @click="cancel()"
                                    >
                                        Go back
                                    </button>
                                </div>
                            </div>
                        </div>
                    </form>
                </Modal>
                <Modal v-model="showThirdModal" title="Reset Failed">
                    <form novalidate>
                        <div class="form-group">
                            <label for="formField1"
                                >Invalid or missing email.<br />
                                Please input existing email address.</label
                            >
                        </div>
                        <div class="row modal-footer">
                            <div class="col-sm-12">
                                <div class="float-right">
                                    <button
                                        class="btn btn-primary"
                                        type="button"
                                        @click="cancel()"
                                    >
                                        Go back
                                    </button>
                                </div>
                            </div>
                        </div>
                    </form>
                </Modal>
            </ClientOnly>
        </div>
    </div>
</template>

<script>
import { getAuth, sendPasswordResetEmail } from 'firebase/auth';

export default {
    name: 'SignUpForm',
    data() {
        return {
            email: '',
            showSecondModal: false,
            showThirdModal: false,
        };
    },
    methods: {
        cancel() {
            this.showSecondModal = false;
            this.showThirdModal = false;
        },
        async reset() {
            const auth = getAuth();
            if (this.email === '') {
                this.showThirdModal = true;
                return;
            }
            sendPasswordResetEmail(auth, this.email)
                .then(() => {
                    this.showSecondModal = true;
                    console.log('email sent');
                })
                .catch(() => {
                    this.showThirdModal = true;
                    console.log('error sending email');
                });
            this.email = '';
        },
    },
};
</script>

<style scoped>
* {
    box-sizing: border-box;
}

body {
    background: #f6f5f7;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    height: 100vh;
    margin: -20px 0 50px;
}

h1 {
    font-weight: bold;
    margin: 0;
}

h2 {
    text-align: center;
}

p {
    font-size: 14px;
    font-weight: 100;
    line-height: 20px;
    letter-spacing: 0.5px;
    margin: 20px 0 30px;
}

span {
    font-size: 12px;
}

a {
    color: #333;
    font-size: 14px;
    text-decoration: none;
    margin: 15px 0;
}

button {
    border-radius: 20px;
    border: 1px solid #6495ed;
    background-color: #6495ed;
    color: white;
    font-size: 12px;
    font-weight: bold;
    padding: 12px 45px;
    letter-spacing: 1px;
    text-transform: uppercase;
    transition: transform 80ms ease-in;
}

button:active {
    transform: scale(0.95);
}

button:focus {
    outline: none;
}

button.ghost {
    background-color: transparent;
    border-color: white;
}

form {
    background-color: #ffffff;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    padding: 0 50px;
    height: 100%;
    text-align: center;
}

input {
    background-color: #eee;
    border: none;
    padding: 12px 15px;
    margin: 8px 0;
    width: 100%;
}

.container {
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
    position: relative;
    overflow: hidden;
    width: 768px;
    max-width: 100%;
    min-height: 480px;
}

.form-container {
    position: absolute;
    top: 0;
    height: 100%;
    transition: all 0.6s ease-in-out;
}

.sign-in-container {
    left: 0;
    width: 50%;
    z-index: 2;
}

.container.right-panel-active .sign-in-container {
    transform: translateX(100%);
}

.sign-up-container {
    left: 0;
    width: 50%;
    opacity: 0;
    z-index: 1;
}

.container.right-panel-active .sign-up-container {
    transform: translateX(100%);
    opacity: 1;
    z-index: 5;
    animation: show 0.6s;
}

@keyframes show {
    0%,
    49.99% {
        opacity: 0;
        z-index: 1;
    }

    50%,
    100% {
        opacity: 1;
        z-index: 5;
    }
}

.overlay-container {
    position: absolute;
    top: 0;
    left: 50%;
    width: 50%;
    height: 100%;
    overflow: hidden;
    transition: transform 0.6s ease-in-out;
    z-index: 100;
}

.container.right-panel-active .overlay-container {
    transform: translateX(-100%);
}

.overlay {
    background: #6495ed;
    background: -webkit-linear-gradient(to right, white, white);
    background: linear-gradient(to right, white, white);
    background-repeat: no-repeat;
    background-size: cover;
    background-position: 0 0;
    color: #ffffff;
    position: relative;
    left: -100%;
    height: 100%;
    width: 200%;
    transform: translateX(0);
    transition: transform 0.6s ease-in-out;
}

.container.right-panel-active .overlay {
    transform: translateX(50%);
}

.overlay-panel {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    padding: 0 40px;
    text-align: center;
    top: 0;
    height: 100%;
    width: 50%;
    transform: translateX(0);
    transition: transform 0.6s ease-in-out;
}

.overlay-left {
    transform: translateX(-20%);
}

.container.right-panel-active .overlay-left {
    transform: translateX(0);
}

.overlay-right {
    right: 0;
    transform: translateX(0);
}

.container.right-panel-active .overlay-right {
    transform: translateX(20%);
}

.social-container {
    margin: 20px 0;
}

.social-container a {
    border: 1px solid #dddddd;
    border-radius: 50%;
    display: inline-flex;
    justify-content: center;
    align-items: center;
    margin: 0 5px;
    height: 40px;
    width: 40px;
}

footer {
    background-color: #222;
    color: #fff;
    font-size: 14px;
    bottom: 0;
    position: fixed;
    left: 0;
    right: 0;
    text-align: center;
    z-index: 999;
}
</style>
