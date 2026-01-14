<div id="dentix-timer-container" style="max-width: 400px; margin: 20px auto; padding: 20px; border-radius: 20px; background: #f0f9ff; border: 2px solid #007bff; text-align: center; font-family: sans-serif;">
    <h3 style="color: #0056b3; margin-top: 0;">🦷 Chrono-Brossage</h3>
    
    <div id="timer-display" style="font-size: 3rem; font-weight: bold; color: #007bff; margin: 15px 0;">02:00</div>
    
    <div id="timer-instruction" style="min-height: 50px; font-weight: 600; color: #444; margin-bottom: 20px;">
        Prêt pour des dents de super-héros ?
    </div>

    <div style="display: flex; gap: 10px; justify-content: center;">
        <button id="startBtn" onclick="toggleTimer()" style="background: #28a745; color: white; border: none; padding: 12px 25px; border-radius: 50px; cursor: pointer; font-size: 1rem; font-weight: bold;">
            Démarrer !
        </button>
        <button onclick="resetTimer()" style="background: #6c757d; color: white; border: none; padding: 12px 25px; border-radius: 50px; cursor: pointer; font-size: 1rem;">
            Réinitialiser
        </button>
    </div>
</div>

<script>
    let timeLeft = 120; // 2 minutes
    let timerId = null;
    const display = document.getElementById('timer-display');
    const instruction = document.getElementById('timer-instruction');
    const startBtn = document.getElementById('startBtn');

    const steps = [
        { time: 120, text: "En bas à gauche : on brosse bien partout ! ⬇️" },
        { time: 90, text: "En bas à droite : fais briller ces dents ! ⬇️" },
        { time: 60, text: "En haut à gauche : on n'oublie pas l'arrière ! ⬆️" },
        { time: 30, text: "En haut à droite : dernière ligne droite ! ⬆️" }
    ];

    function updateDisplay() {
        let minutes = Math.floor(timeLeft / 60);
        let seconds = timeLeft % 60;
        display.innerText = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        
        // Mise à jour de l'instruction
        const currentStep = steps.find(s => timeLeft >= s.time - 30 && timeLeft <= s.time);
        if (currentStep && timeLeft > 0) {
            instruction.innerText = currentStep.text;
        }
    }

    function toggleTimer() {
        if (timerId) {
            clearInterval(timerId);
            timerId = null;
            startBtn.innerText = "Reprendre";
            startBtn.style.background = "#28a745";
        } else {
            startBtn.innerText = "Pause";
            startBtn.style.background = "#ffc107";
            timerId = setInterval(() => {
                timeLeft--;
                updateDisplay();
                if (timeLeft <= 0) {
                    clearInterval(timerId);
                    instruction.innerText = "Félicitations ! Tes dents sont étincelantes ! ✨";
                    instruction.style.color = "#28a745";
                    startBtn.style.display = "none";
                }
            }, 1000);
        }
    }

    function resetTimer() {
        clearInterval(timerId);
        timerId = null;
        timeLeft = 120;
        updateDisplay();
        instruction.innerText = "Prêt pour des dents de super-héros ?";
        instruction.style.color = "#444";
        startBtn.style.display = "block";
        startBtn.innerText = "Démarrer !";
        startBtn.style.background = "#28a745";
    }
</script>
