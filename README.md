<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsive Register and Typing Animation</title>
    <style>
        /* Google Font */
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap');

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Poppins', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: #fff;
            overflow: hidden;
        }

        .container {
            background: rgba(255, 255, 255, 0.95);
            padding: 2rem;
            border-radius: 12px;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.3);
            max-width: 450px;
            width: 100%;
            text-align: center;
            position: relative;
        }

        h2 {
            font-size: 2rem;
            color: #2575fc;
            font-weight: 700;
            margin-bottom: 1.5rem;
        }

        label {
            display: block;
            font-weight: 500;
            margin-bottom: 0.5rem;
            color: #333;
        }

        input[type="text"] {
            width: 100%;
            padding: 15px;
            margin-bottom: 1rem;
            border: none;
            border-radius: 8px;
            background: #f4f4f4;
            font-size: 1rem;
            transition: all 0.3s;
            box-shadow: inset 0 3px 6px rgba(0, 0, 0, 0.1);
        }

        input[type="text"]:focus {
            background: #fff;
            outline: none;
            box-shadow: 0 3px 6px rgba(0, 0, 0, 0.1), 0 0 15px rgba(37, 117, 252, 0.5);
        }

        .cookies-info {
            display: flex;
            align-items: center;
            justify-content: flex-start;
            margin-bottom: 1.5rem;
        }

        input[type="checkbox"] {
            margin-right: 10px;
            cursor: pointer;
            accent-color: #2575fc;
        }

        .btn {
            width: 100%;
            padding: 15px;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: #fff;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }

        .btn:disabled {
            background: #ccc;
            cursor: not-allowed;
            box-shadow: none;
        }

        .btn:hover:not(:disabled) {
            box-shadow: 0 10px 30px rgba(37, 117, 252, 0.5);
            transform: translateY(-3px);
        }

        .btn:active {
            transform: translateY(0);
        }

        /* Typing Animation */
        .welcome-message {
            display: none;
            font-size: 2.5rem; /* Memperbesar teks welcome */
            font-weight: 700;
            color: #fff;
            border-right: 3px solid;
            white-space: nowrap;
            overflow: hidden;
            width: 0;
            margin-top: 20px;
            word-wrap: break-word;
            word-break: break-word;
        }

        /* Mulai Belajar Button */
        .start-learning {
            display: none;
            margin-top: 20px;
            padding: 10px 20px;
            background-color: #2575fc;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1.2rem;
        }

        .start-learning:hover {
            background-color: #6a11cb;
        }

        @keyframes typing {
            from { width: 0; }
            to { width: 100%; }
        }

        @keyframes blink {
            50% { border-color: transparent; }
        }

        /* Responsif */
        @media (max-width: 768px) {
            .container {
                padding: 1.5rem;
            }

            h2 {
                font-size: 1.8rem;
            }

            .btn {
                font-size: 0.9rem;
            }

            input[type="text"] {
                font-size: 0.9rem;
            }

            .welcome-message {
                font-size: 2rem;  /* Memperbesar juga di layar kecil */
                width: 100%;  /* Pastikan teks selalu membungkus dengan baik */
            }
        }

        @media (max-width: 480px) {
            .container {
                padding: 1.2rem;
            }

            h2 {
                font-size: 1.5rem;
            }

            .btn {
                font-size: 0.8rem;
            }

            input[type="text"] {
                font-size: 0.85rem;
            }

            .welcome-message {
                font-size: 1.8rem;  /* Teks tetap besar meski di layar kecil */
                width: 100%;  /* Teks akan menyesuaikan dengan ukuran layar */
            }
        }

        /* Pop-up styles */
        .popup {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            display: flex;
            justify-content: center;
            align-items: center;
            visibility: hidden;
            opacity: 0;
            transition: visibility 0s, opacity 0.5s;
            z-index: 10;
        }

        .popup-content {
            background-color: white;
            padding: 2rem;
            border-radius: 10px;
            text-align: center;
            max-width: 400px;
            width: 80%;
            color: #333;
        }

        .popup-content h3 {
            margin-bottom: 1rem;
        }

        .popup-content button {
            padding: 10px 20px;
            background-color: #2575fc;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1rem;
        }

        .popup-content button:hover {
            background-color: #6a11cb;
        }

        .popup.show {
            visibility: visible;
            opacity: 1;
        }
    </style>
</head>
<body>

<!-- Pop-up -->
<div class="popup" id="popup">
    <div class="popup-content">
        <h3>Halo Pengguna Baru!</h3>
        <p>Mohon untuk mengisi registrasi dulu yah.</p>
        <button id="closePopup">Oke</button>
    </div>
</div>

<div class="container" id="formContainer">
    <h2>Register Now</h2>
    <form id="registerForm">
        <label for="username">Username</label>
        <input type="text" id="username" name="username" required placeholder="Enter your username...">
        
        <div class="cookies-info">
            <input type="checkbox" id="acceptCookies">
            <label for="acceptCookies">I accept the cookies policy</label>
        </div>

        <button type="submit" class="btn" id="registerButton" disabled>Register</button>
    </form>
</div>

<!-- Welcome Message Section -->
<div class="welcome-message" id="welcomeMessage"></div>
<button class="start-learning" id="startLearningButton">Mulai Belajar</button>

<script>
    const acceptCookiesCheckbox = document.getElementById('acceptCookies');
    const registerButton = document.getElementById('registerButton');
    const registerForm = document.getElementById('registerForm');
    const formContainer = document.getElementById('formContainer');
    const welcomeMessage = document.getElementById('welcomeMessage');
    const startLearningButton = document.getElementById('startLearningButton');
    const popup = document.getElementById('popup');
    const closePopup = document.getElementById('closePopup');

    // Show popup when page loads
    window.onload = function() {
        popup.classList.add('show');
    }

    // Close popup when button is clicked
    closePopup.addEventListener('click', function() {
        popup.classList.remove('show');
    });acceptCookiesCheckbox.addEventListener('change', function() {
        registerButton.disabled = !acceptCookiesCheckbox.checked;
    });

    registerForm.addEventListener('submit', function(event) {
        event.preventDefault();  // Prevent form from submitting

        // Ambil username dari input
        const username = document.getElementById('username').value;

        // Sembunyikan form setelah submit
        formContainer.style.display = 'none';

        // Tampilkan pesan selamat datang dengan efek ketik satu per satu
        let text = `Halo ${username}, selamat datang di website kami yang didukung oleh Kemendikbud untuk membantu Anda belajar!`;
        welcomeMessage.style.display = 'block';
        welcomeMessage.style.whiteSpace = 'normal'; // Memungkinkan teks untuk membungkus
        typeText(welcomeMessage, text, 0);
    });

    function typeText(element, text, index) {
        if (index < text.length) {
            element.textContent += text.charAt(index);
            setTimeout(() => typeText(element, text, index + 1), 100); // Setiap huruf muncul per 100ms
        } else {
            // Tampilkan tombol "Mulai Belajar" setelah teks selesai diketik
            startLearningButton.style.display = 'block';
        }
    }

    // Event listener untuk tombol "Mulai Belajar"
    startLearningButton.addEventListener('click', function() {
        window.location.href = 'video.html'; // Ganti '#' dengan URL halaman yang diinginkan
    });
</script>

</body>
</html>
