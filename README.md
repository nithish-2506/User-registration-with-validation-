<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>User Registration</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
    }

    form {
      max-width: 400px;
      margin: auto;
    }

    input {
      display: block;
      width: 100%;
      margin-bottom: 12px;
      padding: 10px;
      font-size: 16px;
    }

    .error {
      color: red;
      font-size: 14px;
      margin-bottom: 10px;
    }

    button {
      padding: 10px 20px;
    }
  </style>
</head>
<body>
  <h2>User Registration</h2>
  <form id="registerForm">
    <input type="text" id="username" placeholder="Username" required />
    <input type="email" id="email" placeholder="Email" required />
    <input type="password" id="password" placeholder="Password" required />
    <input type="password" id="confirmPassword" placeholder="Confirm Password" required />
    <div id="errorMsg" class="error"></div>
    <button type="submit">Register</button>
  </form>

  <script>
    const form = document.getElementById('registerForm');
    const errorMsg = document.getElementById('errorMsg');

    form.addEventListener('submit', (e) => {
      e.preventDefault();
      const username = document.getElementById('username').value.trim();
      const email = document.getElementById('email').value.trim();
      const password = document.getElementById('password').value;
      const confirmPassword = document.getElementById('confirmPassword').value;

      errorMsg.textContent = '';

      // Basic validations
      if (!username || !email || !password || !confirmPassword) {
        errorMsg.textContent = 'All fields are required.';
        return;
      }

      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      if (!emailRegex.test(email)) {
        errorMsg.textContent = 'Invalid email format.';
        return;
      }

      if (password.length < 6) {
        errorMsg.textContent = 'Password must be at least 6 characters.';
        return;
      }

      if (password !== confirmPassword) {
        errorMsg.textContent = 'Passwords do not match.';
        return;
      }

      // Submit to backend (optional)
      fetch('/api/register', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username, email, password })
      })
      .then(res => res.json())
      .then(data => {
        if (data.success) {
          alert('User registered successfully!');
          form.reset();
        } else {
          errorMsg.textContent = data.message || 'Registration failed.';
        }
      })
      .catch(() => {
        errorMsg.textContent = 'Error connecting to server.';
      });
    });
  </script>
</body>
</html>
