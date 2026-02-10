<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sug'urta Bilimdonlari Platformasi - Professional</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .gradient-bg { background: linear-gradient(135deg, #0f172a 0%, #1e40af 100%); }
        .option-btn { transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1); }
    </style>
</head>
<body class="bg-slate-50 min-h-screen flex flex-col text-slate-900">

    <!-- Header -->
    <header class="gradient-bg text-white py-8 shadow-2xl">
        <div class="container mx-auto px-4 text-center">
            <h1 class="text-3xl md:text-4xl font-extrabold tracking-tight">Professional Sug'urta Testi</h1>
            <p class="mt-3 opacity-80 text-base max-w-lg mx-auto italic">O'zbekiston Respublikasining "Sug'urta faoliyati to'g'risida"gi Qonuni va normativ hujjatlar asosida</p>
        </div>
    </header>

    <main class="flex-grow container mx-auto px-4 py-8 max-w-3xl">
        <!-- Start Screen -->
        <div id="start-screen" class="bg-white rounded-3xl shadow-2xl p-10 text-center border border-slate-100">
            <div class="mb-8">
                <div class="w-24 h-24 bg-blue-50 text-blue-700 rounded-3xl flex items-center justify-center mx-auto mb-6 transform rotate-3 shadow-inner">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
                    </svg>
                </div>
                <h2 class="text-3xl font-bold text-slate-800">Bilimingizni sinovdan o'tkazing</h2>
                <div class="text-slate-600 mt-4 space-y-2 text-left bg-slate-50 p-6 rounded-2xl border border-slate-200">
                    <p>• Test 10 ta tasodifiy murakkab savoldan iborat.</p>
                    <p>• Har bir savolga faqat bitta to'g'ri javob mavjud.</p>
                    <p>• "Sug'urta faoliyati to'g'risida"gi O'RQ-730 sonli Qonun talablari inobatga olingan.</p>
                </div>
            </div>
            <button onclick="startQuiz()" class="group relative w-full inline-flex items-center justify-center px-8 py-4 font-bold text-white transition-all duration-200 bg-blue-600 font-pj rounded-xl focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-900 hover:bg-blue-700">
                Imtihonni boshlash
            </button>
        </div>

        <!-- Quiz Screen -->
        <div id="quiz-screen" class="hidden">
            <div class="bg-white rounded-3xl shadow-2xl p-8 border border-slate-100">
                <div class="flex justify-between items-center mb-8">
                    <div>
                        <span id="progress-text" class="text-xs font-bold text-blue-600 uppercase tracking-widest">Savol: 1 / 10</span>
                        <h4 class="text-slate-400 text-xs mt-1">Hayot sug'urtasi tarmog'i</h4>
                    </div>
                    <div class="h-3 w-40 bg-slate-100 rounded-full overflow-hidden border border-slate-200">
                        <div id="progress-bar" class="h-full bg-blue-600 transition-all duration-500 ease-out" style="width: 10%"></div>
                    </div>
                </div>

                <h3 id="question-text" class="text-xl md:text-2xl font-bold text-slate-800 mb-10 leading-snug">
                    Savol yuklanmoqda...
                </h3>

                <div id="options-container" class="space-y-4">
                    <!-- Savollar shu yerda -->
                </div>

                <div class="mt-10 pt-6 border-t border-slate-100 flex justify-between items-center">
                    <span class="text-slate-400 text-sm italic">Javobni belgilang va keyingisiga o'ting</span>
                    <button id="next-btn" onclick="nextQuestion()" disabled class="bg-slate-200 text-slate-400 cursor-not-allowed font-bold py-3 px-10 rounded-xl transition-all duration-300 transform active:scale-95">
                        Keyingisi
                    </button>
                </div>
            </div>
        </div>

        <!-- Result Screen -->
        <div id="result-screen" class="hidden text-center bg-white rounded-3xl shadow-2xl p-10 border border-slate-100">
            <h2 class="text-3xl font-black text-slate-800 mb-4">Sertifikat Natijasi</h2>
            <div id="score-circle" class="relative w-48 h-48 mx-auto my-8">
                <svg class="w-full h-full" viewBox="0 0 36 36">
                    <path class="text-slate-100" stroke-width="3" stroke="currentColor" fill="none" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" />
                    <path id="score-stroke" class="text-blue-600" stroke-width="3" stroke-dasharray="0, 100" stroke-linecap="round" stroke="currentColor" fill="none" d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831" />
                </svg>
                <div class="absolute inset-0 flex flex-col items-center justify-center">
                    <span id="final-score" class="text-5xl font-black text-slate-800 leading-none">0/10</span>
                    <span class="text-xs font-bold text-slate-400 uppercase mt-2">To'g'ri javob</span>
                </div>
            </div>
            <p id="result-message" class="text-lg text-slate-600 mb-10 px-4 font-medium italic"></p>
            <button onclick="resetQuiz()" class="w-full bg-slate-900 hover:bg-black text-white font-bold py-4 px-8 rounded-2xl transition duration-300 shadow-xl">
                Qayta urinish
            </button>
        </div>
    </main>

    <footer class="py-10 text-center border-t border-slate-200 mt-auto bg-white">
        <p class="text-slate-400 text-sm">&copy; 2026 O'zbekiston Professional Sug'urta Mutaxassislari Platformasi</p>
    </footer>

    <script>
        const allQuestions = [
            {
                q: "Qaysi klass 'Sug'urta qildiruvchining foydada ishtirok etishi bilan hayotni sug'urta qilish' deb ataladi?",
                options: ["I klass", "II klass", "III klass", "IV klass"],
                correct: 2 // III klass
            },
            {
                q: "O'zbekiston qonunchiligiga ko'ra, hayot sug'urtasi shartnomasining eng kam muddati qancha bo'lishi kerak?",
                options: ["6 oy", "1 yil", "3 yil", "Muddat belgilanmagan"],
                correct: 1 
            },
            {
                q: "Hayot sug'urtasi bo'yicha VII-klass (Kapitalni qaytarish bilan sug'urtalash) asosiy o'ziga xosligi nimada?",
                options: ["Faqat sog'liqni saqlashga yo'naltirilganligi", "To'langan mukofotlar va kafolatlangan summaning qaytarilishi", "Faqat vafot etganda to'lov amalga oshirilishi", "Majburiy sug'urta turi hisoblanishi"],
                correct: 1
            },
            {
                q: "Jismoniy shaxs hayot sug'urtasi uchun to'lagan badallari bo'yicha qanday soliq imtiyoziga ega?",
                options: ["Soliq imtiyozi mavjud emas", "Badallar summasi daromad solig'idan (JShDS) ozod etiladi", "Faqat 10% chegirma beriladi", "Faqat pensiya yoshidagilar uchun imtiyoz bor"],
                correct: 1
            },
            {
                q: "Sug'urta zaxiralari qaysi organ tomonidan belgilangan me'yorlar asosida joylashtiriladi?",
                options: ["Markaziy bank", "Istiqbolli loyihalar milliy agentligi (NAPP)", "Soliq qo'mitasi", "Moliya vazirligi"],
                correct: 1
            },
            {
                q: "Annuitet sug'urtasi (V-klass) shartnomasi bo'yicha to'lovlar qanday ko'rinishda bo'ladi?",
                options: ["Faqat bir marta beriladigan summa", "Davriy ravishda (oylik/choraklik) to'lanadigan renta", "Faqat davolanish xarajatlarini qoplash", "Zarar aniqlangandan keyin to'lanadigan kompensatsiya"],
                correct: 1
            },
            {
                q: "Quyidagilarning qaysi biri hayot sug'urtasi bo'yicha professional ishtirokchi hisoblanmaydi?",
                options: ["Sug'urta brokeri", "Sug'urta agenti", "Sug'urta aktuariysi", "Tijorat banki xazinachisi"],
                correct: 3
            },
            {
                q: "Hayot sug'urtasida 'Sog'liqni sug'urta qilish' klassi nechanchi raqam ostida klassifikatsiyalanadi?",
                options: ["IV klass", "VI klass", "I klass", "VII klass"],
                correct: 1 // VI klass
            },
            {
                q: "Agar sug'urta shartnomasida 'Naf oluvchi' ko'rsatilmagan bo'lsa va sug'urtalangan shaxs vafot etsa, to'lov kimga beriladi?",
                options: ["Davlat byudjetiga", "Sug'urta kompaniyasining ixtiyorida qoladi", "Qonuniy merosxo'rlarga", "Sug'urta agentiga"],
                correct: 2
            },
            {
                q: "Qayta sug'urtalash (reinsurance) operatsiyasining asosiy maqsadi nima?",
                options: ["Yangi mijozlarni jalb qilish", "Moliyaviy barqarorlikni ta'minlash va xavfni taqsimlash", "Soliqlardan qochish", "Sug'urta tariflarini oshirish"],
                correct: 1
            },
            {
                q: "Tillararo (Unit-linked) hayot sug'urtasi shartnomalarida investitsiya xavfi kimning zimmasida bo'ladi?",
                options: ["Faqat sug'urta kompaniyasi", "Faqat davlat", "Sug'urta qildiruvchi (mijoz)", "Hech kimning"],
                correct: 2
            },
            {
                q: "Sug'urta hodisasi yuz berganda to'lovni rad etish uchun asosiy sabab nima bo'lishi mumkin?",
                options: ["Mijozning yashash joyi o'zgarishi", "Mijoz tomonidan qasddan qilingan harakat yoki noto'g'ri ma'lumot berilishi", "Kompaniya foydasining kamayishi", "Sug'urta agentining ishdan ketishi"],
                correct: 1
            }
        ];

        let currentQuestions = [];
        let currentQuestionIndex = 0;
        let score = 0;
        let selectedOption = null;

        function startQuiz() {
            currentQuestions = [...allQuestions].sort(() => 0.5 - Math.random()).slice(0, 10);
            currentQuestionIndex = 0;
            score = 0;
            
            document.getElementById('start-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            
            showQuestion();
        }

        function showQuestion() {
            const question = currentQuestions[currentQuestionIndex];
            selectedOption = null;
            
            document.getElementById('progress-text').innerText = `Savol: ${currentQuestionIndex + 1} / 10`;
            document.getElementById('progress-bar').style.width = `${(currentQuestionIndex + 1) * 10}%`;
            document.getElementById('question-text').innerText = question.q;
            
            const optionsContainer = document.getElementById('options-container');
            optionsContainer.innerHTML = '';
            
            question.options.forEach((opt, index) => {
                const btn = document.createElement('button');
                btn.className = "option-btn w-full text-left p-5 border-2 border-slate-100 rounded-2xl hover:border-blue-400 hover:bg-blue-50 flex items-center group relative overflow-hidden";
                btn.innerHTML = `
                    <div class="w-10 h-10 rounded-xl bg-slate-50 text-slate-400 flex items-center justify-center mr-5 group-hover:bg-blue-600 group-hover:text-white font-black text-sm transition-all duration-200 border border-slate-200">
                        ${String.fromCharCode(65 + index)}
                    </div>
                    <span class="text-slate-700 font-semibold text-base md:text-lg pr-4">${opt}</span>
                `;
                btn.onclick = () => selectOption(index, btn);
                optionsContainer.appendChild(btn);
            });
            
            const nextBtn = document.getElementById('next-btn');
            nextBtn.disabled = true;
            nextBtn.className = "bg-slate-200 text-slate-400 cursor-not-allowed font-bold py-3 px-10 rounded-xl transition-all duration-300";
        }

        function selectOption(index, element) {
            selectedOption = index;
            
            const buttons = document.getElementById('options-container').children;
            for (let btn of buttons) {
                btn.classList.remove('border-blue-600', 'bg-blue-50', 'ring-4', 'ring-blue-100');
                btn.querySelector('div').classList.remove('bg-blue-600', 'text-white', 'border-blue-600');
                btn.querySelector('div').classList.add('bg-slate-50', 'text-slate-400');
            }
            
            element.classList.add('border-blue-600', 'bg-blue-50', 'ring-4', 'ring-blue-100');
            element.querySelector('div').classList.remove('bg-slate-50', 'text-slate-400');
            element.querySelector('div').classList.add('bg-blue-600', 'text-white', 'border-blue-600');
            
            const nextBtn = document.getElementById('next-btn');
            nextBtn.disabled = false;
            nextBtn.className = "bg-blue-600 text-white hover:bg-blue-700 shadow-lg shadow-blue-200 font-bold py-3 px-10 rounded-xl transition-all duration-300 transform active:scale-95";
        }

        function nextQuestion() {
            if (selectedOption === currentQuestions[currentQuestionIndex].correct) {
                score++;
            }
            
            currentQuestionIndex++;
            
            if (currentQuestionIndex < currentQuestions.length) {
                showQuestion();
            } else {
                showResult();
            }
        }

        function showResult() {
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.remove('hidden');
            
            document.getElementById('final-score').innerText = `${score}/10`;
            document.getElementById('score-stroke').setAttribute('stroke-dasharray', `${score * 10}, 100`);
            
            let message = "";
            if (score === 10) message = "Fenomenal natija! Siz O'zbekiston sug'urta qonunchiligini mukammal bilasiz.";
            else if (score >= 8) message = "A'lo natija! Siz professional darajadagi mutaxassissiz.";
            else if (score >= 6) message = "Yaxshi, ammo ba'zi qonuniy jihatlarni takrorlash foydadan xoli emas.";
            else message = "Natija past. 'Sug'urta faoliyati to'g'risida'gi Qonunni (O'RQ-730) diqqat bilan o'qib chiqishni tavsiya etamiz.";
            
            document.getElementById('result-message').innerText = message;
        }

        function resetQuiz() {
            startQuiz();
        }
    </script>
</body>
</html>
