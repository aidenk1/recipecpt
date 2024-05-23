---
layout: default
permalink: /fullRecipe
title: fullrecipe
---
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Full Recipe</title>
    <style>
        body {
            background: linear-gradient(45deg, #000000, #2c2c2e);
            height: 100vh;
            font-family: 'Montserrat', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            margin: 0;
            padding: 20px;
        }
        .container {
            background-color: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 600px;
            text-align: center; /* Center-align the text */
        }
        img {
            width: 200px; /* Adjust the width as needed */
            height: auto;
            border-radius: 10px;
            margin-bottom: 20px;
        }
        #backButton,
        #likeButton {
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            padding: 10px 20px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s ease;
            margin-top: 20px; /* Add space above the button */
        }
        #backButton:hover,
        #likeButton:hover {
            background-color: #45a049;
        }
        #likeCount {
            color: #FFD700; /* Golden color for the like count */
            font-weight: bold;
            margin-top: 10px;
        }
    </style>
</head>

<body>
    <div class="container" id="recipeContainer">
        <h2 id="recipeName"></h2>
        <img id="recipeThumbnail" src="" alt="Recipe Thumbnail">
        <h3>Instructions</h3>
        <p id="recipeInstructions"></p>
        <h3>Ingredients</h3>
        <p id="recipeIngredients"></p>
        <h3>Supplies</h3>
        <p id="recommendedSupplies"></p>
        <p id="postedBy"></p>
        <button id="likeButton">Like</button>
        <p id="likeCount"></p>
        <button id="backButton">Back to Homepage</button>
    </div>
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const recipe = JSON.parse(localStorage.getItem('fullRecipe'));
            if (recipe) {
                document.getElementById('recipeName').textContent = recipe.name;
                // Check if the thumbnail URL starts with 'data:image/png;base64,'
                if (recipe.thumbnail.startsWith('data:image/png;base64,')) {
                    // If it does, set the src attribute directly to the base64 image data
                    document.getElementById('recipeThumbnail').src = recipe.thumbnail;
                } else {
                    // Otherwise, treat it as a regular image URL
                    document.getElementById('recipeThumbnail').src = recipe.thumbnail;
                }
                document.getElementById('recipeInstructions').textContent = recipe.instruction;
                document.getElementById('recipeIngredients').textContent = recipe.ingredients;
                document.getElementById('recommendedSupplies').textContent = recipe.supplies;
                document.getElementById('postedBy').textContent = `Posted by: ${recipe.userid}`;
                document.getElementById('likeCount').textContent = `Likes: ${recipe.likes}`;
                document.getElementById('likeButton').addEventListener('click', async () => {
                    console.log(recipe.id)
                    const response = await fetch("http://127.0.0.1:8086/api/recipe/", {
                        method: 'PUT'
                        headers: {
                            'Content-Type': 'application/json',
                            'Access-Control-Allow-Origin': 'http://127.0.0.1:4100',
                            'Access-Control-Allow-Credentials': 'true'
                        },
                        body: JSON.stringify({ "recipe_id": recipe.id })
                    });
                });
            } else {
                document.getElementById('recipeContainer').textContent = 'Recipe not found';
            }
            // Add event listener to the back button
            document.getElementById('backButton').addEventListener('click', () => {
                window.location.href = './homepage'; // Redirect to the homepage
            });
        });
    </script>
</body>

</html>