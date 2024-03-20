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
