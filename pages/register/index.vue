<template>
    <div class="register-page">
        <form @submit.prevent="register" class="w-full max-w-sm">
            <h1 class="text-center text-3xl font-bold mb-6">Register</h1>
            <div class="mb-4 text-black">
                <label for="username" class="block text-lg font-medium">Username:</label>
                <input type="text" v-model="username" id="username"
                    class="w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
                    required />
            </div>
            <div class="mb-4 text-black">
                <label for="email" class="block text-lg font-medium">Email:</label>
                <input type="email" v-model="email" id="email"
                    class="w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
                    required />
            </div>
            <div class="mb-6 text-black">
                <label for="password" class="block text-lg font-medium">Password:</label>
                <input type="password" v-model="password" id="password"
                    class="w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
                    required />
            </div>
            <button type="submit"
                class="w-full bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                Register
            </button>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            username: '',
            email: '',
            password: ''
        };
    },
    methods: {
        async register() {
            const response = await fetch('http://localhost:3003/register', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json', // Make sure this header is set to 'application/json'
                },
                body: JSON.stringify({
                    username: this.username,
                    email: this.email,
                    password: this.password
                }),
            });

        }
    }
};
</script>

<style scoped>
.register-page {
    background: linear-gradient(165deg, #003f5b, #5f5195, #cc4c91, #ff6f4e, #dda500, #9bb02c, #65b05d, #47a884); /* made at https://learnui.design/tools/gradient-generator.html */
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    padding: 20px;
}

form {
    /* background-color: white; */
    backdrop-filter: blur(100px);
    padding: 30px;
    box-shadow: inset 0 0 10px rgba(4, 95, 168, 0.646);
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

h1 {
    font-family: 'Arial', sans-serif;
    color: #333;
}

label {
    display: block;
    margin-bottom: 8px;
    font-size: 1rem;
    font-weight: 500;
    color: #333;
}

input {
    width: 100%;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
    margin-bottom: 15px;
}

button {
    background-color: #3498db;
    color: white;
    font-weight: bold;
    padding: 12px;
    border-radius: 5px;
    border: none;
    width: 100%;
    cursor: pointer;
}

button:hover {
    background-color: #2980b9;
}
</style>
