# Crop-Monitoring-App with AI
This crop monitoring and advisory website offers a convenient, innovative solution for farmers to manage crop health and respond to changing weather conditions in real-time by leveraging digital technologies, farmers can improve their yields and respond more effectively to environmental challenges.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Crop Monitoring App with Weather Advisory</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #e9ecef;
            margin: 0;
            padding: 0;
        }

        .container {
            max-width: 800px;
            margin: 20px auto;
            background-color: #ffffff;
            padding: 30px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
            text-align: center;
        }

        h1 {
            color: #007bff;
            margin-bottom: 20px;
        }

        .upload-section, .weather-info, .advisory-section {
            margin-bottom: 20px;
        }

        .upload-label {
            display: block;
            font-size: 18px;
            margin-bottom: 10px;
            color: #333;
        }

        input[type="file"] {
            display: block;
            margin: 0 auto;
        }

        .image-preview {
            margin-top: 15px;
            display: none;
            text-align: center;
        }

        .image-preview img {
            max-width: 100%;
            max-height: 300px;
            border-radius: 10px;
        }

        button {
            display: block;
            width: 100%;
            padding: 10px;
            background-color: #007bff;
            color: white;
            font-size: 18px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 15px;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: #0056b3;
        }

        .weather-info, .advisory-section {
            background-color: #f8f9fa;
            padding: 15px;
            border-radius: 5px;
        }

        .advisory {
            padding: 10px;
            background-color: #d4edda;
            border-left: 5px solid #28a745;
            margin-top: 10px;
        }

        #analysisResult {
            font-size: 20px;
            font-weight: bold;
            color: #333;
        }

        @media (max-width: 600px) {
            .container {
                padding: 15px;
            }

            button {
                font-size: 16px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Crop Monitoring App with Weather Advisory</h1>

        <div class="upload-section">
            <label for="cropImage" class="upload-label">Upload Crop Image:</label>
            <input type="file" id="cropImage" accept="image/*" onchange="previewImage(event)">
            <div class="image-preview" id="imagePreview"></div>
        </div>

        <button onclick="analyzeImage()">Analyze Image</button>

        <div class="weather-info">
            <h2>Weather in Your Area:</h2>
            <p id="weatherData">Fetching weather...</p>
        </div>

        <div class="advisory-section">
            <h2>Crop Advisory</h2>
            <div id="advisory" class="advisory">
                Upload an image to get a weather-based advisory for your crops.
            </div>
        </div>

        <div class="result-section" id="resultSection">
            <h2>Analysis Result</h2>
            <p id="analysisResult">Upload an image to see the result.</p>
        </div>
    </div>

    <script>
        // Function to preview image
        function previewImage(event) {
            const imagePreview = document.getElementById("imagePreview");
            const file = event.target.files[0];
            const reader = new FileReader();

            reader.onload = function() {
                const img = document.createElement("img");
                img.src = reader.result;
                imagePreview.innerHTML = "";
                imagePreview.appendChild(img);
                imagePreview.style.display = "block";
            };

            if (file) {
                reader.readAsDataURL(file);
            }
        }

        // Function to analyze the image (Mock)
        function analyzeImage() {
            const resultSection = document.getElementById("resultSection");
            const analysisResult = document.getElementById("analysisResult");
            
            if (!document.getElementById("cropImage").files.length) {
                alert("Please upload an image first!");
                return;
            }

            // Mock analysis result
            const randomResults = [
                "Healthy Crop",
                "Diseased: Rust Disease Detected",
                "Nutrient Deficiency: Nitrogen Deficiency Detected",
                "Pest Infestation Detected"
            ];

            const randomIndex = Math.floor(Math.random() * randomResults.length);
            const result = randomResults[randomIndex];
            
            // Display result
            analysisResult.innerText = `Result: ${result}`;
            resultSection.style.display = "block";
        }

        // Function to fetch weather data
        function getWeather() {
            // Fetching weather from a public API (replace with real API key)
            fetch("https://api.openweathermap.org/data/2.5/weather?q=Delhi&appid=75cbd0863d89f79753227ba028d651c0&units=metric")
            .then(response => response.json())
            .then(data => {
                const temp = data.main.temp;
                const conditions = data.weather[0].description;
                weatherData.innerHTML = `Temperature: ${temp}°C, Conditions: ${conditions}`;
                updateAdvisory(temp, conditions);
            })
            .catch(error => {
                weatherData.innerHTML = "Unable to fetch weather data.";
            });
        }

        // Function to update crop advisory based on weather
        function updateAdvisory(temp, conditions) {
            if (temp > 35) {
                advisory.innerHTML = "High temperatures detected. Ensure adequate water supply to prevent drought stress.";
            } else if (conditions.includes("rain")) {
                advisory.innerHTML = "Rain is expected. Monitor your crop for possible fungal diseases and avoid overwatering.";
            } else {
                advisory.innerHTML = "Weather conditions are optimal for crop growth.";
            }
        }

        // Initialize by getting weather data
        getWeather();
    </script>
</body>
</html>

