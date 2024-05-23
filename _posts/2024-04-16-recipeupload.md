---
layout: default
permalink: /upload
title: recipe upload
---
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Upload Recipe</title>
    <style>
        body {
            background: linear-gradient(45deg, #000000, #2c2c2e);
            height: 100vh;
            font-family: 'Montserrat', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0;
        }

        .container {
            background-color: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            width: 80%;
            max-width: 500px;
        }

        input[type="text"],
        textarea {
            width: 100%;
            padding: 10px;
            margin-bottom: 20px;
            border-radius: 5px;
            border: none;
            background-color: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 16px;
        }

        button {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            background-color: #4CAF50;
            color: white;
            font-size: 16px;
            cursor: pointer;
            transition: background-color 0.3s ease;
            width: 100%;
        }

        button:hover {
            background-color: #45a049;
        }

        .message {
            color: white;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Upload Your Recipe</h2>
        <form id="recipeForm">
            <input type="text" id="recipeName" name="recipeName" placeholder="Recipe Name" required><br>
            <textarea id="recipeInstructions" name="recipeInstructions" rows="4" placeholder="Recipe Instructions" required></textarea><br>
            <textarea id="recipeIngredients" name="recipeIngredients" rows="4" placeholder="Recipe Ingredients" required></textarea><br>
            <textarea id="recommendedSupplies" name="recommendedSupplies" rows="4" placeholder="Recommended Supplies" required></textarea><br>
            <input type="text" id="thumbnailUrl" name="thumbnailUrl" placeholder="Image URL" required><br>
            <button type="submit">Upload Recipe</button>
        </form>
        <div class="message" id="message"></div>
    </div>

    <script>
        async function submitRecipeForm(event) {
            event.preventDefault(); // Prevent the form from submitting the default way

            const recipeName = document.getElementById('recipeName').value;
            const recipeInstructions = document.getElementById('recipeInstructions').value;
            const recipeIngredients = document.getElementById('recipeIngredients').value;
            const recommendedSupplies = document.getElementById('recommendedSupplies').value;
            const thumbnailUrl = document.getElementById('thumbnailUrl').value;

            const recipeData = {
                userid: localStorage.getItem('userId'), // Assuming the userId is stored in localStorage
                recipeName: recipeName,
                recipeInstructions: recipeInstructions,
                recipeIngredients: recipeIngredients,
                recommendedSupplies: recommendedSupplies,
                recipeThumbnail: thumbnailUrl
            };

            try {
                const apiUrl = "http://127.0.0.1:8086/api/recipe/";
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Access-Control-Allow-Origin': 'http://127.0.0.1:4100',
                        'Access-Control-Allow-Credentials': 'true'
                    },
                    body: JSON.stringify(recipeData)
                });

                if (!response.ok) {
                    throw new Error('Recipe upload request was not successful');
                }

                const responseData = await response.json();
                document.getElementById('message').innerText = 'Recipe uploaded successfully.';
                // Redirect to another page after 2 seconds
                setTimeout(() => {
                    window.location.href = './homepage';
                }, 2000);
            } catch (error) {
                console.error('Error:', error);
                document.getElementById('message').innerText = 'Failed to upload recipe. Please try again.';
            }
        }

        document.getElementById('recipeForm').addEventListener('submit', submitRecipeForm);
    </script>
</body>
</html>
