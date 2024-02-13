<template>
  <div class="auth-container">
    <!-- Login Section -->
    <div v-if="!authenticated && currentSection === 'login'" class="auth-section">
      <h2>Login</h2>
      <form @submit.prevent="login" class="auth-form">
        <label for="username" class="input-label">Username:</label>
        <input type="text" id="username" v-model="username" required class="input" />
        <label for="password" class="input-label">Password:</label>
        <input
          id="loginPassword"
          v-model="password"
          :type="showPassword ? 'text' : 'password'"
          required
          class="input"
        />
        <label for="showPassword" class="checkbox-label">
          <input type="checkbox" id="showPassword" v-model="showPassword" class="checkbox" />
          Show Password
        </label>
        <button type="submit" class="button">Login</button>
      </form>

      <!-- Additional content after login -->
      <section v-if="authenticated" class="auth-section">
        <h2>Welcome, {{ username }}!</h2>
        <!-- Display additional information or content -->
        <p>This is your personalized dashboard. You can do XYZ here.</p>
        <!-- Output button -->
        <button @click="showOutput" class="button">Show Output</button>
        <!-- ... other content ... -->
        <button @click="logout" class="button">Logout</button>
      </section><br>

      <!-- Navigation Buttons -->
      <div class="navigation-buttons">
        <button @click="goToCreateAccount" class="nav-button">Create Account</button><br><br>
        <button @click="goToForgotPassword" class="nav-button">Forgot Password</button>
      </div>
    </div>

    <!-- Create Account Section -->
    <div v-if="!authenticated && currentSection === 'createAccount'" class="auth-section">
      <h2>Create Account</h2>
      <form @submit.prevent="createAccount" class="auth-form">
        <label for="newUsername" class="input-label">New Username:</label>
        <input type="text" id="newUsername" v-model="newUsername" required class="input" />
        <label for="newPassword" class="input-label">New Password:</label>
        <input
          id="newPassword"
          v-model="newPassword"
          :type="showNewPassword ? 'text' : 'password'"
          required
          class="input"
        />
        <label for="showNewPassword" class="checkbox-label">
          <input type="checkbox" id="showNewPassword" v-model="showNewPassword" class="checkbox" />
          Show Password
        </label>
        <button type="submit" class="button">Create Account</button>
      </form><br>
      <button @click="goToLogin" class="back-button">Back to Login</button>
    </div>

    <!-- Forgot Password Section -->
    <div v-if="!authenticated && currentSection === 'forgotPassword'" class="auth-section">
      <h2>Forgot Password</h2>
      <form @submit.prevent="forgotPassword" class="auth-form">
        <label for="forgotUsername" class="input-label">Username:</label>
        <input type="text" id="forgotUsername" v-model="forgotUsername" required class="input" />
        <label for="newPassword" class="input-label">New Password:</label>
        <input
          id="newPassword"
          v-model="newPassword"
          :type="showNewPassword ? 'text' : 'password'"
          required
          class="input"
        />
        <label for="showNewPassword" class="checkbox-label">
          <input type="checkbox" id="showNewPassword" v-model="showNewPassword" class="checkbox" />
          Show New Password
        </label>
        <button type="submit" class="button">Reset Password</button>
      </form><br>
      <button @click="goToLogin" class="back-button">Back to Login</button>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const username = ref('');
    const password = ref('');
    const newUsername = ref('');
    const newPassword = ref('');
    const showNewPassword = ref(false);
    const authenticated = ref(false);
    const showPassword = ref(false);
    const currentSection = ref('login');
    const forgotUsername = ref('');
    const users = ref([{ username: 'user', password: 'password' }]); // Include the 'users' array

    function login() {
      const isAuthenticated = users.value.some(
        user => user.username === username.value && user.password === password.value
      );

      if (isAuthenticated) {
        authenticated.value = true;
      } else {
        alert('Invalid credentials. If you don\'t have an account, consider creating one.');
      }
    }

    function showOutput() {
      alert('Output button clicked! You can customize this action.');
    }

    function logout() { // Corrected the function name
      alert('you want to logout from your account')
      currentSection.value = 'login';
    }

    function createAccount() {
      if (users.value.some(user => user.username === newUsername.value)) {
        alert('Username already taken. Please choose a different one.');
      } else {
        const newUser = { username: newUsername.value, password: newPassword.value };
        users.value.push(newUser);
        alert('Account created successfully! You can now log in.');
        // Reset the form fields
        newUsername.value = '';
        newPassword.value = '';
        showNewPassword.value = false;
        // Switch to the login section
        currentSection.value = 'login';
      }
    }

    function forgotPassword() {
      const usernameToFind = forgotUsername.value || '';
      const userToReset = users.value.find(user => user.username === usernameToFind);

      if (userToReset) {
        // Update the user's password to the new password
        userToReset.password = newPassword.value;
        alert(`Password reset for ${forgotUsername.value}.`);
        // Reset the form fields
        forgotUsername.value = '';
        newPassword.value = '';
        showNewPassword.value = false;
      } else {
        alert(`User ${forgotUsername.value} not found. Please check the username.`);
      }
    }

    function goToLogin() {
      currentSection.value = 'login';
    }

    function goToCreateAccount() {
      currentSection.value = 'createAccount';
    }

    function goToForgotPassword() {
      currentSection.value = 'forgotPassword';
    }

    return {
      username,
      password,
      authenticated,
      login,
      createAccount,
      showPassword,
      newUsername,
      newPassword,
      showNewPassword,
      currentSection,
      forgotUsername,
      forgotPassword,
      goToLogin,
      goToCreateAccount,
      goToForgotPassword,
      users,
      showOutput,
      logout
    };
  },
};
</script>

<style scoped>
.auth-container {
  display: flex;
  background-image: url('https://img.freepik.com/free-photo/beautiful-shot-forest-with-tall-green-trees_181624-20615.jpg?w=1380&t=st=1707818969~exp=1707819569~hmac=8c3fe69f9c2456d285310b0d1d5d0c293e7bcf3f2047de53825759e4a026b1f6');
  background-size: cover;
  background-position: center;
  overflow: hidden;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-image 0.3s ease-in-out;
}

.auth-section {
  width: 400px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  background-color: rgba(35, 92, 57, 0.9); /* Semi-transparent white background */
  transition: transform 0.3s ease-in-out;
}

.auth-section h2 {
  color: #fff;
  text-align: center;
}

.auth-section .input-label {
  display: block;
  margin-bottom: 8px;
  color: #fff;
}

.auth-section .input {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  box-sizing: border-box;
}

.auth-section .checkbox-label {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
  color: #fff;
}

.auth-section .checkbox {
  margin-right: 8px;
}

.auth-section .button {
  width: 100%;
  padding: 8px 16px;
  background-color: #050a05;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
}

.auth-section .button:hover {
  background-color: #092d1f;
}

.auth-section .button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.auth-section .nav-button,
.auth-section .back-button {
  /* Define common styles for navigation buttons */
  width: 100%;
  padding: 8px 16px;
  background-color: #050a05;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
  margin-bottom: 8px;
}

.auth-section .nav-button:hover,
.auth-section .back-button:hover {
  background-color: #092d1f;
}
</style>
