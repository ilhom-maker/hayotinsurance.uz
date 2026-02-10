<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HayotInsurance.uz - Professional Test Tizimi</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; }
        .gradient-bg { background: linear-gradient(135deg, #0f172a 0%, #1e3a8a 100%); }
        .option-btn { transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1); }
        .custom-glass { background: rgba(255, 255, 255, 0.98); backdrop-filter: blur(10px); }
        .report-card { transition: all 0.3s ease; }
    </style>
</head>
<body class="bg-slate-50 min-h-screen flex flex-col text-slate-900">

    <!-- Header -->
    <header class="gradient-bg text-white py-12 shadow-2xl relative overflow-hidden">
        <div class="container mx-auto px-4 text-center relative z-10">
            <h1 class="text-4xl md:text-5xl font-extrabold tracking-tight mb-4">HayotInsurance.uz</h1>
            <p class="text-blue-100 text-lg md:text-xl max-w-2xl mx-auto font-medium opacity-90">
                Sug'urta bozori mutaxassislari uchun professional malaka imtihoniga tayyorlov platformasi
            </p>
        </div>
    </header>

    <main class="flex-grow container mx-auto px-4 py-12 max-w-4xl -mt-10">
        
        <!-- Start Screen -->
        <div id="start-screen" class="custom-glass rounded-[2.5rem] shadow-2xl p-8 md:p-12 text-center border border-white relative z-20">
            <div class="max-w-md mx-auto">
                <div class="w-20 h-20 bg-blue-600 text-white rounded-2xl flex items-center justify-center mx-auto mb-8 shadow-xl transform -rotate-3">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04m18.236 0a11.955 11.955 0 01-8.618 3.04m0 0V9m0 0a9 9 0 010 18v-2.118" />
                    </svg>
                </div>
                <h2 class="text-3xl font-bold text-slate-800 mb-6 tracking-tight">Bilim darajangizni sinab ko'ring</h2>
                
                <div class="space-y-4 text-left mb-10 bg-slate-50 p-6 rounded-2xl border border-slate-200">
                    <div class="flex items-start">
                        <span class="text-blue-600 mr-3 mt-1 font-bold">✓</span>
                        <p class="text-slate-600 text-sm">Savollar yangi tahrirdagi O'RQ-730 Qonuni asosida.</p>
                    </div>
                    <div class="flex items-start">
                        <span class="text-blue-600 mr-3 mt-1 font-bold">✓</span>
                        <p class="text-slate-600 text-sm">Xatolar ustida ishlash uchun batafsil hisobot.</p>
                    </div>
                </div>

                <button onclick="startQuiz()" class="w-full py-5 bg-blue-600 hover:bg-blue-700 text-white font-bold rounded-2xl shadow-lg shadow-blue-200 transition-all transform hover:-translate-y-1 active:scale-95 text-xl">
                    Testni boshlash
                </button>
            </div>
        </div>

        <!-- Quiz Screen -->
        <div id="quiz-screen" class="hidden">
            <div class="custom-glass rounded-[2.5rem] shadow-2xl p-6 md:p-10 border border-white">
                <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-10 gap-4">
                    <div>
                        <span id="progress-text" class="text-sm font-bold text-blue-600 uppercase tracking-widest bg-blue-50 px-3 py-1 rounded-full border border-blue-100">Savol: 1 / 10</span>
                        <h4 class="text-slate-400 text-xs mt-3 font-semibold uppercase tracking-wider">Yo'nalish: Hayot sug'urtasi (Life)</h4>
                    </div>
                    <div class="w-full md:w-64">
                        <div class="h-2 bg-slate-100 rounded-full overflow-hidden border border-slate-200">
                            <div id="progress-bar" class="h-full bg-blue-600 transition-all duration-700" style="width: 10%"></div>
                        </div>
                    </div>
                </div>

                <div class="min-h-[120px] mb-10">
                    <h3 id="question-text" class="text-2xl md:text-3xl font-bold text-slate-800 leading-tight">
                        Yuklanmoqda...
                    </h3>
                </div>

                <div id="options-container" class="grid grid-cols-1 gap-4">
                    <!-- Options injected here -->
                </div>

                <div class="mt-12 pt-8 border-t border-slate-100 flex justify-end">
                    <button id="next-btn" onclick="nextQuestion()" disabled class="bg-slate-200 text-slate-400 cursor-not-allowed font-bold py-4 px-12 rounded-2xl transition-all duration-300 text-lg">
                        Keyingisi
                    </button>
                </div>
            </div>
        </div>

        <!-- Result Screen -->
        <div id="result-screen" class="hidden">
            <div class="text-center custom-glass rounded-[2.5rem] shadow-2xl p-10 md:p-16 border border-white mb-8">
                <h2 class="text-4xl font-black text-slate-800 mb-2 italic">Imtihon Yakunlandi</h2>
                <div class="relative w-56 h-56 mx-auto my-10">
                    <svg class="w-full h-full transform -rotate-90" viewBox="0 0 36 36">
                        <circle cx="18" cy="18" r="16" fill="none" class="text-slate-100" stroke-width="3" stroke="currentColor"></circle>
                        <circle id="score-stroke" cx="18" cy="18" r="16" fill="none" class="text-blue-600" stroke-width="3" stroke-dasharray="0, 100" stroke-linecap="round" stroke="currentColor" style="transition: stroke-dasharray 1.5s ease-in-out"></circle>
                    </svg>
                    <div class="absolute inset-0 flex flex-col items-center justify-center">
                        <span id="final-score" class="text-6xl font-black text-slate-800">0/10</span>
                        <span id="score-percent" class="text-sm font-bold text-slate-400 mt-2 tracking-widest">0% NATIJA</span>
                    </div>
                </div>
                <div class="max-w-md mx-auto">
                    <p id="result-message" class="text-xl text-slate-700 mb-10 font-medium italic"></p>
                    <button onclick="resetQuiz()" class="w-full bg-slate-900 hover:bg-black text-white font-bold py-5 px-10 rounded-2xl transition duration-300 shadow-2xl text-lg mb-4">
                        Qayta imtihon topshirish
                    </button>
                    <a href="#detailed-report" class="text-blue-600 font-bold hover:underline block">Xatolar tahlilini ko'rish ↓</a>
                </div>
            </div>

            <!-- Detailed Report Section -->
            <div id="detailed-report" class="bg-white rounded-[2rem] shadow-xl p-6 md:p-10 border border-slate-100 overflow-hidden">
                <h3 class="text-2xl font-bold text-slate-800 mb-8 flex items-center">
                    <span class="w-8 h-8 bg-blue-100 text-blue-600 rounded-lg flex items-center justify-center mr-3 text-base">📊</span>
                    Savollar tahlili
                </h3>
                <div id="report-container" class="space-y-6">
                    <!-- Report items injected here -->
                </div>
            </div>
        </div>
    </main>

    <footer class="py-10 text-center text-slate-400 text-sm mt-auto bg-white border-t border-slate-100">
        <p class="font-bold text-slate-500 uppercase tracking-[0.3em] mb-2">HayotInsurance.uz</p>
        <p>© 2026 Barcha huquqlar himoyalangan.</p>
    </footer>

    <script>
        const allQuestions = [
            {
                q: "O'zbekiston Respublikasining 'Sug'urta faoliyati to'g'risida'gi yangi tahrirdagi Qonuni qachon qabul qilingan?",
                options: ["2019-yil", "2020-yil", "2021-yil", "2022-yil"],
                correct: 2 
            },
            {
                q: "Hayot sug'urtasi tarmog'ida III-klass (Foydada ishtirok etuvchi sug'urta) shartnomalarida investitsiya daromadi qanday taqsimlanadi?",
                options: ["Faqat kompaniya foydasiga", "Sug'urtalovchi va sug'urta qildiruvchi o'rtasida shartnoma asosida", "Faqat davlat byudjetiga", "Bunday klassda foyda taqsimlanmaydi"],
                correct: 1
            },
            {
                q: "O'zbekistonda sug'urta bozorini tartibga soluvchi vakolatli organ nomi?",
                options: ["Markaziy Bank", "Moliya Vazirligi", "Istiqbolli loyihalar milliy agentligi (NAPP)", "Savdo-sanoat palatasi"],
                correct: 2
            },
            {
                q: "Hayot sug'urtasi shartnomasi bo'yicha 'Sog'lomlashtirish davri' (Free look period) necha kunni tashkil etishi shart?",
                options: ["5 kun", "10 kun", "14 kun", "30 kun"],
                correct: 2 
            },
            {
                q: "Unit-linked (I-klassga yaqin investitsion sug'urta) mahsulotlarida mijoz nimaga ega bo'ladi?",
                options: ["Faqat kafolatlangan summa", "Sug'urta himoyasi va investitsiya fondlaridagi ulush", "Faqat bank kreditiga", "Hech qanday himoyaga ega bo'lmaydi"],
                correct: 1
            },
            {
                q: "Sug'urta agenti bir vaqtning o'zida nechta hayot sug'urtasi kompaniyasi bilan shartnoma tuzishi mumkin (eksklyuzivlik bo'lmasa)?",
                options: ["Cheklanmagan", "Faqat bitta", "Faqat uchtagacha", "Agentlar hayot sug'urtasida ishlay olmaydi"],
                correct: 1
            },
            {
                q: "V-klass (Annuitetlar) bo'yicha sug'urta to'lovlarining asosiy xususiyati nima?",
                options: ["Bir martalik yirik to'lov", "Muntazam ravishda (renta) to'lanadigan to'lovlar", "Faqat qarindoshlarga beriladigan to'lov", "Sug'urta mukofotini qaytarib berish"],
                correct: 1
            },
            {
                q: "Sug'urta tarmog'idagi 'Aktuariy' kim?",
                options: ["Sug'urta polisi sotuvchisi", "Matematik va statistik usullar bilan xavf hamda tariflarni hisoblovchi mutaxassis", "Kompaniya advokati", "Mijozlarni qabul qiluvchi xodim"],
                correct: 1
            },
            {
                q: "Hayot sug'urtasida 'Surrender Value' (Viykupnaya summa) nima?",
                options: ["Agentning mukofot puli", "Shartnoma muddatidan oldin bekor qilinganda mijozga qaytariladigan to'plangan mablag' qismi", "Soliq jarimasi", "Kompaniya sof foydasi"],
                correct: 1
            },
            {
                q: "JShDS (Daromad solig'i) bo'yicha imtiyoz qo'llanilishi uchun hayot sug'urtasi shartnomasi qancha muddatga tuzilgan bo'lishi shart?",
                options: ["Kamida 6 oy", "Kamida 12 oy", "Kamida 3 yil", "Muddat ahamiyatga ega emas"],
                correct: 1
            }
        ];

        let currentQuestions = [];
        let currentQuestionIndex = 0;
        let score = 0;
        let selectedOption = null;
        let userAnswers = []; // Foydalanuvchi javoblarini saqlash uchun

        function startQuiz() {
            currentQuestions = [...allQuestions].sort(() => 0.5 - Math.random());
            currentQuestionIndex = 0;
            score = 0;
            userAnswers = [];
            
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
                btn.className = "option-btn w-full text-left p-6 border-2 border-slate-100 rounded-2xl hover:border-blue-500 hover:bg-blue-50 flex items-center group relative";
                btn.innerHTML = `
                    <div class="w-10 h-10 rounded-lg bg-slate-100 text-slate-500 flex items-center justify-center mr-6 group-hover:bg-blue-600 group-hover:text-white font-bold transition-all border border-slate-200">
                        ${String.fromCharCode(65 + index)}
                    </div>
                    <span class="text-slate-700 font-bold text-lg flex-1">${opt}</span>
                `;
                btn.onclick = () => selectOption(index, btn);
                optionsContainer.appendChild(btn);
            });
            
            const nextBtn = document.getElementById('next-btn');
            nextBtn.disabled = true;
            nextBtn.className = "bg-slate-200 text-slate-400 cursor-not-allowed font-bold py-4 px-12 rounded-2xl transition-all duration-300 text-lg";
        }

        function selectOption(index, element) {
            selectedOption = index;
            
            const buttons = document.getElementById('options-container').children;
            for (let btn of buttons) {
                btn.classList.remove('border-blue-600', 'bg-blue-50', 'ring-4', 'ring-blue-100');
                btn.querySelector('div').classList.remove('bg-blue-600', 'text-white', 'border-blue-600');
                btn.querySelector('div').classList.add('bg-slate-100', 'text-slate-500');
            }
            
            element.classList.add('border-blue-600', 'bg-blue-50', 'ring-4', 'ring-blue-100');
            element.querySelector('div').classList.remove('bg-slate-100', 'text-slate-500');
            element.querySelector('div').classList.add('bg-blue-600', 'text-white', 'border-blue-600');
            
            const nextBtn = document.getElementById('next-btn');
            nextBtn.disabled = false;
            nextBtn.className = "bg-blue-600 text-white hover:bg-blue-700 shadow-xl font-bold py-4 px-12 rounded-2xl transition-all duration-300 transform active:scale-95 text-lg";
        }

        function nextQuestion() {
            // Javobni saqlash
            userAnswers.push(selectedOption);

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
            const percent = score * 10;
            document.getElementById('score-percent').innerText = `${percent}% NATIJA`;
            
            setTimeout(() => {
                document.getElementById('score-stroke').setAttribute('stroke-dasharray', `${percent}, 100`);
            }, 100);
            
            let message = "";
            if (score === 10) message = "Fenomenal natija! Siz haqiqiy sug'urta ekspertsiz.";
            else if (score >= 7) message = "Juda yaxshi natija! Bilimingiz professional faoliyat uchun yetarli.";
            else message = "Bilimingizni oshirishingiz kerak. O'RQ-730 sonli qonunni qayta o'qib chiqish tavsiya etiladi.";
            
            document.getElementById('result-message').innerText = message;

            // Batafsil hisobotni shakllantirish
            generateReport();
        }

        function generateReport() {
            const reportContainer = document.getElementById('report-container');
            reportContainer.innerHTML = '';

            currentQuestions.forEach((question, index) => {
                const userChoice = userAnswers[index];
                const isCorrect = userChoice === question.correct;

                const item = document.createElement('div');
                item.className = `p-6 rounded-2xl border-l-8 ${isCorrect ? 'bg-green-50 border-green-500' : 'bg-red-50 border-red-500'} report-card`;
                
                item.innerHTML = `
                    <div class="flex items-center mb-3">
                        <span class="text-xs font-black px-2 py-1 rounded ${isCorrect ? 'bg-green-200 text-green-700' : 'bg-red-200 text-red-700'} uppercase mr-3">
                            ${isCorrect ? 'Tog\'ri' : 'Xato'}
                        </span>
                        <span class="text-slate-400 font-bold text-sm">Savol #${index + 1}</span>
                    </div>
                    <p class="text-slate-800 font-bold mb-4">${question.q}</p>
                    <div class="space-y-2">
                        <div class="text-sm">
                            <span class="text-slate-500">Sizning javobingiz:</span> 
                            <span class="font-bold ${isCorrect ? 'text-green-600' : 'text-red-600'}">${question.options[userChoice]}</span>
                        </div>
                        ${!isCorrect ? `
                        <div class="text-sm">
                            <span class="text-slate-500">To'g'ri javob:</span> 
                            <span class="font-bold text-green-600">${question.options[question.correct]}</span>
                        </div>
                        ` : ''}
                    </div>
                `;
                reportContainer.appendChild(item);
            });
        }

        function resetQuiz() {
            startQuiz();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }
    </script>
</body>
</html>
