---
layout: default
permalink: /homepage
title: homepage
---
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Homepage</title>
    <style>
        body {
            background: linear-gradient(45deg, #000000, #2c2c2e);
            height: 100vh;
            font-family: 'Montserrat', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            color: white;
            margin: 0;
        }

        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        input[type="text"] {
            padding: 10px;
            border-radius: 5px;
            border: none;
            margin-bottom: 20px;
            width: 300px;
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
        }

        button:hover {
            background-color: #45a049;
        }

        .recipe-item {
            display: flex;
            align-items: center;
            margin: 10px 0;
            background: rgba(255, 255, 255, 0.1);
            padding: 10px;
            border-radius: 5px;
            width: 90%;
            max-width: 600px;
        }

        .recipe-item img {
            width: 100px;
            height: auto;
            border-radius: 5px;
            margin-right: 10px;
            cursor: pointer;
        }

        .recipe-item span {
            font-size: 16px;
        }

        .like-count {
            margin-left: 10px;
            color: #ccc;
        }

        #backToTop {
            display: none;
            position: fixed;
            bottom: 30px;
            right: 30px;
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            z-index: 1000;
        }

        #backToTop:hover {
            background-color: #45a049;
        }
    </style>
</head>

<body>
    <div class="container">
        <h1>Welcome to Recipe Search</h1>
        <form id="recipeForm">
            <input type="text" placeholder="Search for recipes...">
            <button type="submit">Search Recipe</button>
        </form>
        <a href="./upload"><button>Upload Your Own Recipe</button></a>
        <ol id="results"></ol>
    </div>
    <button id="backToTop" onclick="scrollToTop()">Back to Top</button>
    <script>
        const recipeForm = document.getElementById('recipeForm');
        recipeForm.addEventListener('submit', async (event) => {
            event.preventDefault();
            try {
                const apiUrl = "http://127.0.0.1:8086/api/recipe";
                const response = await fetch(apiUrl);
                const recipes = await response.json();
                // Clear existing results
                const resultsList = document.getElementById('results');
                resultsList.innerHTML = '';
                // Filter and display recipes with a specific name
                const searchInput = recipeForm.querySelector('input[type="text"]').value.trim().toLowerCase();
                recipes.forEach(recipe => {
                    if (recipe.name.toLowerCase().includes(searchInput)) {
                        const listItem = document.createElement('li');
                        listItem.className = 'recipe-item';

                        const thumbnail = document.createElement('img');
                        thumbnail.src = recipe.thumbnail;
                        thumbnail.alt = recipe.name;
                        thumbnail.addEventListener('click', () => {
                            showFullRecipe(recipe);
                        });

                        const recipeName = document.createElement('span');
                        recipeName.textContent = recipe.name;

                        const likeCount = document.createElement('span');
                        likeCount.className = 'like-count';
                        likeCount.textContent = `Likes: ${recipe.likes}`;

                        listItem.appendChild(thumbnail);
                        listItem.appendChild(recipeName);
                        listItem.appendChild(likeCount);
                        resultsList.appendChild(listItem);
                    }
                });
                // Show the "Back to Top" button
                document.getElementById('backToTop').style.display = 'block';
            } catch (error) {
                console.error('Error loading recipes:', error);
            }
        });

        function showFullRecipe(recipe) {
            // Save the recipe data to localStorage
            localStorage.setItem('fullRecipe', JSON.stringify(recipe));
            // Redirect to the full recipe page
            window.location.href = './fullRecipe';
        }

        window.addEventListener('scroll', () => {
            const backToTopButton = document.getElementById('backToTop');
            if (window.pageYOffset > 100) {
                backToTopButton.style.display = 'block';
            } else {
                backToTopButton.style.display = 'none';
            }
        });

        function scrollToTop() {
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }
    </script>
</body>

</html>