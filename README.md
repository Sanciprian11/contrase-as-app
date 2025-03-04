<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Generador de Contraseñas Seguras</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 20px auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
            color: #333;
        }
        .form-group {
            margin: 15px 0;
        }
        button {
            background: #007bff;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            margin: 5px;
        }
        button:hover {
            background: #0056b3;
        }
        #password-output {
            background: #f0f0f0;
            padding: 10px;
            margin: 10px 0;
            word-break: break-all;
            border-radius: 4px;
        }
        #strength {
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Generador de Contraseñas</h1>
        <div class="form-group">
            <label for="length">Longitud (8-32):</label>
            <input type="number" id="length" min="8" max="32" value="12">
        </div>
        <div class="form-group">
            <label><input type="checkbox" id="uppercase" checked> Mayúsculas</label>
            <label><input type="checkbox" id="lowercase" checked> Minúsculas</label>
            <label><input type="checkbox" id="numbers" checked> Números</label>
            <label><input type="checkbox" id="symbols" checked> Símbolos</label>
        </div>
        <button id="generate">Generar</button>
        <button id="copy">Copiar</button>
        <div id="password-output"></div>
        <div id="strength"></div>
    </div>

    <script>
        const chars = {
            uppercase: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
            lowercase: 'abcdefghijklmnopqrstuvwxyz',
            numbers: '0123456789',
            symbols: '!@#$%^&*()_+[]{}|;:,.<>?'
        };

        const passwordOutput = document.getElementById('password-output');
        const strengthOutput = document.getElementById('strength');
        const lengthInput = document.getElementById('length');
        const uppercaseCheck = document.getElementById('uppercase');
        const lowercaseCheck = document.getElementById('lowercase');
        const numbersCheck = document.getElementById('numbers');
        const symbolsCheck = document.getElementById('symbols');
        const generateBtn = document.getElementById('generate');
        const copyBtn = document.getElementById('copy');

        function generatePassword() {
            const length = Math.max(8, Math.min(32, parseInt(lengthInput.value)));
            let availableChars = '';
            
            if (uppercaseCheck.checked) availableChars += chars.uppercase;
            if (lowercaseCheck.checked) availableChars += chars.lowercase;
            if (numbersCheck.checked) availableChars += chars.numbers;
            if (symbolsCheck.checked) availableChars += chars.symbols;

            if (!availableChars) {
                alert('Por favor, selecciona al menos un tipo de carácter');
                return;
            }

            let password = '';
            for (let i = 0; i < length; i++) {
                const randomIndex = Math.floor(Math.random() * availableChars.length);
                password += availableChars[randomIndex];
            }

            passwordOutput.textContent = password;
            evaluateStrength(password);
        }

        function evaluateStrength(password) {
            let strength = 'Débil';
            let color = '#dc3545';
            const hasUpper = /[A-Z]/.test(password);
            const hasLower = /[a-z]/.test(password);
            const hasNumbers = /[0-9]/.test(password);
            const hasSymbols = /[!@#$%^&*()_+\[\]{}|;:,.<>?]/.test(password);
            const variety = [hasUpper, hasLower, hasNumbers, hasSymbols].filter(Boolean).length;

            if (password.length >= 12 && variety >= 3) {
                strength = 'Fuerte';
                color = '#28a745';
            } else if (password.length >= 8 && variety >= 2) {
                strength = 'Media';
                color = '#ffc107';
            }

            strengthOutput.textContent = `Fuerza: ${strength}`;
            strengthOutput.style.color = color;
        }

        async function copyToClipboard() {
            if (!passwordOutput.textContent) return;
            try {
                await navigator.clipboard.writeText(passwordOutput.textContent);
                copyBtn.textContent = '¡Copiado!';
                setTimeout(() => copyBtn.textContent = 'Copiar', 2000);
            } catch (err) {
                console.error('Error al copiar:', err);
            }
        }

        generateBtn.addEventListener('click', generatePassword);
        copyBtn.addEventListener('click', copyToClipboard);
    </script>
</body>
</html>
