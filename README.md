<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>Marauder's Map - Ink Effect</title>
<style>
    body {
        margin: 0;
        font-family: "Arial", sans-serif;
        background-color: #1b1b1b;
        color: #fff;
        text-align: center;
    }
    #start-screen, #end-screen {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 100vh;
        background: #3e2c1c;
        color: #f5e6c4;
    }
    #map-container {
        display: none;
        position: relative;
        text-align: center;
        background-color: black;
    }
    #map-image {
        max-width: 100%;
        height: auto;
        opacity: 0;
        animation: revealInk 4s ease-in forwards;
        animation-delay: 0.5s;
        -webkit-mask-image: radial-gradient(circle at center, rgba(0,0,0,1) 0%, transparent 70%);
        -webkit-mask-repeat: no-repeat;
        -webkit-mask-size: 0% 0%;
    }
    @keyframes revealInk {
        0% {
            -webkit-mask-size: 0% 0%;
            opacity: 0;
        }
        50% {
            opacity: 1;
            -webkit-mask-size: 150% 150%;
        }
        100% {
            opacity: 1;
            -webkit-mask-size: 300% 300%;
        }
    }
    .marker {
        position: absolute;
        background: rgba(255,255,255,0.7);
        color: black;
        padding: 5px;
        border-radius: 50%;
        cursor: pointer;
        font-weight: bold;
    }
    .footsteps {
        position: absolute;
        width: 40px;
        height: 40px;
        background: url('https://i.ibb.co/Zm4B2yB/footsteps.png') no-repeat center/contain;
        animation: walk 2s linear forwards;
    }
    @keyframes walk {
        from { opacity: 0; transform: translateY(0px); }
        to { opacity: 1; transform: translateY(-50px); }
    }
    #puzzle-box {
        position: fixed;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background: #3e2c1c;
        padding: 20px;
        border: 2px solid #f5e6c4;
        color: #f5e6c4;
        display: none;
    }
    input[type="text"] {
        padding: 5px;
        margin-top: 10px;
    }
    button {
        margin-top: 10px;
        padding: 5px 10px;
        cursor: pointer;
    }
</style>
</head>
<body>

<audio id="paper-sound" src="https://www.soundjay.com/misc/sounds/page-turn-1.mp3"></audio>
<audio id="steps-sound" src="https://www.soundjay.com/footsteps/footsteps-on-hardwood-1.mp3"></audio>

<div id="start-screen">
    <h1>🪄 I solemnly swear that I am up to no good 🪄</h1>
    <button onclick="showMap()">Show Map</button>
</div>

<div id="map-container">
    <img id="map-image" src="https://i.ibb.co/1zLZk6h/marauders-map.jpg" alt="Marauder's Map">
    <div class="marker" style="top: 40%; left: 30%;" onclick="showPuzzle(0)">M1</div>
    <div class="marker" style="top: 60%; left: 50%;" onclick="showPuzzle(1)">C2</div>
    <div class="marker" style="top: 50%; left: 70%;" onclick="showPuzzle(2)">I3</div>
    <button style="position: absolute; bottom: 10px; right: 10px;" onclick="endMap()">Mischief Managed</button>
</div>

<div id="puzzle-box">
    <p id="puzzle-text"></p>
    <input type="text" id="answer" placeholder="أدخل الإجابة">
    <button onclick="checkAnswer()">تحقق</button>
</div>

<div id="end-screen" style="display:none;">
    <h1>✨ Mischief Managed ✨</h1>
</div>

<script>
    const puzzles = [
        {question: "مكان لا يدخله إلا من يُجب على سؤال حكيم، وبابه بلا كلمة سر بل لغز جديد كل مرة.", answer: "برج رافنكلو"},
        {question: "أستاذة مؤقتة تولت رعاية المخلوقات السحرية عندما غاب معلمها الأصلي، وعلمت الطلاب عن أحادي القرن.", answer: "ويلهيلمينا جروبلي-بلانك"},
        {question: "قطعة ورق قد تبدو فارغة، لكن عند تمرير العصا السحرية وكتابة كلمة سر، تُظهر خريطة كاملة.", answer: "خريطة النهابين"}
    ];

    let currentPuzzleIndex = null;

    function showMap() {
        document.getElementById('paper-sound').play();
        document.getElementById('start-screen').style.display = 'none';
        document.getElementById('map-container').style.display = 'block';
    }

    function showPuzzle(index) {
        currentPuzzleIndex = index;
        document.getElementById('puzzle-text').textContent = puzzles[index].question;
        document.getElementById('puzzle-box').style.display = 'block';
    }

    function checkAnswer() {
        const userAnswer = document.getElementById('answer').value.trim();
        if(userAnswer === puzzles[currentPuzzleIndex].answer) {
            document.querySelectorAll('.marker')[currentPuzzleIndex].textContent = puzzles[currentPuzzleIndex].answer;
            document.getElementById('puzzle-box').style.display = 'none';
            document.getElementById('steps-sound').play();
            showFootsteps(currentPuzzleIndex);
        } else {
            alert("إجابة خاطئة! حاول مرة أخرى.");
        }
    }

    function showFootsteps(index) {
        const marker = document.querySelectorAll('.marker')[index];
        const foot = document.createElement('div');
        foot.classList.add('footsteps');
        foot.style.top = marker.style.top;
        foot.style.left = marker.style.left;
        document.getElementById('map-container').appendChild(foot);
    }

    function endMap() {
        document.getElementById('map-container').style.display = 'none';
        document.getElementById('end-screen').style.display = 'flex';
    }
</script>

</body>
</html>
