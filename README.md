<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>معمل الجرعات السحرية</title>
<style>
    body {
        background-color: #1c1c1c;
        color: #fff;
        font-family: Arial, sans-serif;
        text-align: center;
    }
    h1 {
        margin-top: 20px;
    }
    #lab {
        display: flex;
        justify-content: center;
        margin-top: 20px;
    }
    .shelf {
        display: flex;
        flex-direction: column;
        gap: 10px;
        margin-right: 50px;
    }
    .ingredient {
        width: 80px;
        height: 80px;
        background-color: #444;
        border-radius: 10px;
        cursor: grab;
        border: 2px solid #ccc;
    }
    #cauldron {
        width: 300px;
        height: 200px;
        background-color: #333;
        border-radius: 50% 50% 0 0;
        position: relative;
        overflow: hidden;
        border: 4px solid #777;
    }
    #drop-zone {
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
    }
    #status {
        margin-top: 20px;
        font-size: 20px;
    }
</style>
</head>
<body>

<h1>⚗️ معمل الجرعات السحرية</h1>
<p id="potion-name"></p>

<div id="lab">
    <div class="shelf">
        <div class="ingredient" draggable="true" data-name="عشب التنين" style="background-color: green;"></div>
        <div class="ingredient" draggable="true" data-name="دم العنقاء" style="background-color: red;"></div>
        <div class="ingredient" draggable="true" data-name="أشواك القنفذ" style="background-color: brown;"></div>
    </div>
    <div id="cauldron">
        <div id="drop-zone"></div>
    </div>
</div>

<div id="status"></div>

<script>
    const potions = [
        { name: "جرعة القوة", ingredients: ["عشب التنين", "دم العنقاء"] },
        { name: "جرعة النوم", ingredients: ["أشواك القنفذ", "عشب التنين"] }
    ];

    let currentPotion = potions[Math.floor(Math.random() * potions.length)];
    let chosenIngredients = [];

    document.getElementById("potion-name").textContent = "الجرعة المطلوبة: " + currentPotion.name;

    const ingredients = document.querySelectorAll(".ingredient");
    const dropZone = document.getElementById("drop-zone");

    ingredients.forEach(ingredient => {
        ingredient.addEventListener("dragstart", e => {
            e.dataTransfer.setData("text", ingredient.dataset.name);
        });
    });

    dropZone.addEventListener("dragover", e => {
        e.preventDefault();
    });

    dropZone.addEventListener("drop", e => {
        e.preventDefault();
        const ingredientName = e.dataTransfer.getData("text");
        chosenIngredients.push(ingredientName);

        if (chosenIngredients.length === currentPotion.ingredients.length) {
            if (JSON.stringify(chosenIngredients) === JSON.stringify(currentPotion.ingredients)) {
                document.getElementById("status").textContent = "✨ الجرعة نجحت!";
                document.getElementById("status").style.color = "lightgreen";
            } else {
                document.getElementById("status").textContent = "💥 فشلت الجرعة!";
                document.getElementById("status").style.color = "red";
            }
        }
    });
</script>

</body>
</html>
