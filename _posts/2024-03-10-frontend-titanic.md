<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Titanic Passenger Information</title>
</head>
<body>
    <h1>Titanic Passenger Information</h1>
    <form id="titanicForm">
        <label for="pclass">Ticket class (1 = 1st, 2 = 2nd, 3 = 3rd):</label><br>
        <input type="number" id="pclass" name="pclass" min="1" max="3" required><br>
         <label for="sex">Sex:</label><br>
        <select id="sex" name="sex" required>
            <option value="male">Male</option>
            <option value="female">Female</option>
        </select><br>
         <label for="age">Age (in years):</label><br>
        <input type="number" id="age" name="age" min="0" required><br>
         <label for="sibsp"># of siblings / spouses aboard the Titanic:</label><br>
        <input type="number" id="sibsp" name="sibsp" min="0" required><br>
          <label for="parch"># of parents / children aboard the Titanic:</label><br>
        <input type="number" id="parch" name="parch" min="0" required><br>
          <label for="fare">Passenger fare:</label><br>
        <input type="number" id="fare" name="fare" min="0" required><br>
          <label for="embarked">Embarked:</label><br>
        <select id="embarked" name="embarked" required>
            <option value="A">A</option>
            <option value="B">B</option>
            <option value="C">C</option>
        </select><br>
        <label for="alone">Alone (true or false):</label><br>
        <select id="alone" name="alone" required>
            <option value="true">True</option>
            <option value="false">False</option>
        </select><br>
        <input type="submit" value="Submit">
    </form>

<script>
        document.getElementById('titanicForm').addEventListener('submit', function(event) {
            event.preventDefault(); // Prevent the default form submission

            // Collect form data
            const formData = {
                pclass: document.getElementById('pclass').value,
                sex: document.getElementById('sex').value,
                age: document.getElementById('age').value,
                sibsp: document.getElementById('sibsp').value,
                parch: document.getElementById('parch').value,
                fare: document.getElementById('fare').value,
                embarked: document.getElementById('embarked').value,
                alone: document.getElementById('alone').value
            };

            // Send POST request to the backend API
            fetch('http://localhost:8086/predict', { // Change URL to match your backend
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(formData)
            })
            .then(response => response.json())
            .then(data => {
                // Handle response data here
                console.log(data);
            })
            .catch(error => {
                console.error('Error:', error);
            });
        });
    </script>
</body>
</html>
