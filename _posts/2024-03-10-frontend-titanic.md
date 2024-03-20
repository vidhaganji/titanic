<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Titanic Survival Predictor</title>
    <style>
        /* Your CSS styles here */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #E6E6FA; /* Light Purple */
            overflow-x: hidden;
            position: relative;
        }
        h1 {
            text-align: center;
            margin-bottom: 20px !important;
            color: #fff; /* White text */
        }
        #floating-gif {
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            z-index: -1;
            pointer-events: none;
        }
        h2 {
            text-align: center;
            color: #E6E6FA; /* White text */
            font-size: 28px;
            font-weight: bold;
            font-family: 'Playfair Display', serif;
            text-transform: uppercase;
            letter-spacing: 2px;
            margin-bottom: 20px;
        }
        form {
            max-width: 500px;
            margin: 0 auto;
            background-color: rgba(255, 255, 255, 0.9);
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
        }
        label {
            display: block;
            margin-bottom: 15px;
            color: #0d3b66;
            font-weight: bold;
        }
        input[type="text"],
        input[type="number"],
        select {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
        }
        input[type="checkbox"] {
            margin-bottom: 15px;
        }
        input[type="submit"] {
            background-color: #4B0082;
            color: #fff;
            border: none;
            padding: 15px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }
        input[type="submit"]:hover {
            background-color: #4B0082;
        }
        /* Container for form and GIF */
        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
            margin-bottom: 50px; /* Modified margin */
            font-family: 'Playfair Display', serif; /* Adding Playfair Display font */
            color: #6a0dad; 
        }
    </style>
</head>
<body>
    <h1>Titanic Survival Predictor</h1>
    <form id="titanicForm">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" required><br><br>
        <label for="pclass">Passenger Class:</label>
        <select id="pclass" name="pclass" required>
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
        </select><br><br>
        <label for="sex">Sex:</label>
        <select id="sex" name="sex" required>
            <option value="male">Male</option>
            <option value="female">Female</option>
        </select><br><br>
        <label for="age">Age:</label>
        <input type="number" id="age" name="age" required><br><br>
        <label for="sibsp">Siblings/Spouses Aboard:</label>
        <input type="number" id="sibsp" name="sibsp" required><br><br>
        <label for="parch">Parents/Children Aboard:</label>
        <input type="number" id="parch" name="parch" required><br><br>
        <label for="fare">Fare:</label>
        <input type="number" id="fare" name="fare" required><br><br>
        <label for="embarked">Embarked:</label>
        <select id="embarked" name="embarked" required>
            <option value="C">Cherbourg</option>
            <option value="Q">Queenstown</option>
            <option value="S">Southampton</option>
        </select><br><br>
        <label for="alone">Alone:</label>
        <input type="checkbox" id="alone" name="alone"><br><br>
        <button type="button" onclick="predictSurvival()">Predict Survival</button>
    </form>
    <div id="result"></div>
    <script>
        function predictSurvival() {
            var form = document.getElementById('titanicForm');
            var name = document.getElementById('name');
            var resultDiv = document.getElementById('result');
            var formData = {
                pclass: form['pclass'].value,
                sex: form['sex'].value,
                age: form['age'].value,
                sibsp: form['sibsp'].value,
                parch: form['parch'].value,
                fare: form['fare'].value,
                embarked: form['embarked'].value,
                alone: form['alone'].checked ? 'true' : 'false'
            };
            fetch('http://localhost:8086/api/titanic/predict', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Accept': 'application/json'
                },
                body: JSON.stringify(formData)
            })
            .then(response => response.json())
            .then(data => {
                resultDiv.innerHTML = '<h2>Prediction Result for ' + name.value + '</h2>';
                for (var key in data) {
                    resultDiv.innerHTML += '<p>' + key + ': ' + data[key] + '</p>';
                }
                var deathProbability = parseFloat(data['death_percentage']);
                var survivalProbability = parseFloat(data['survivability_percentage']);
            })
            .catch(error => {
                console.error('Error:', error);
            });
        }
    </script>
</body>
</html>
