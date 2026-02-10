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
        
        .gradient-bg { 
            background: radial-gradient(circle at top right, #1e40af, #0f172a); 
        }
        
        .custom-glass { 
            background: rgba(255, 255, 255, 0.95); 
            backdrop-filter: blur(20px); 
            border: 1px solid rgba(255, 255, 255, 0.3);
        }

        .option-btn { 
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .option-btn:not(.selected):hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
        }

        .correct-anim {
            background-color: #dcfce7 !important;
            border-color: #22c55e !important;
            animation: pulse-green 0.5s;
        }

        .wrong-anim {
            background-color: #fee2e2 !important;
            border-color: #ef4444 !important;
            animation: shake 0.4s;
        }

        @keyframes pulse-green {
            0% { transform: scale(1); }
            50% { transform: scale(1.02); }
            100% { transform: scale(1); }
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            75% { transform: translateX(5px); }
        }

        .progress-glow {
            box-shadow: 0 0 15px rgba(37, 99, 235, 0.5);
        }
    </style>
</head>
<body class="bg-[#f8fafc] min-h-screen flex flex-col text-slate-900">

    <!-- Header -->
    <header class="gradient-bg text-white py-16 shadow-2xl relative overflow-hidden">
        <div class="absolute inset-0 opacity-10">
            <svg class="w-full h-full" viewBox="0 0 100 100" preserveAspectRatio="none">
                <path d="M0 100 C 20 0 50 0 100 100 Z" fill="white"></path>
            </svg>
        </div>
        <div class="container mx-auto px-4 text-center relative z-10">
            <h1 class="text-5xl md:text-6xl font-extrabold tracking-tighter mb-4 animate-fade-in">
                Hayot<span class="text-blue-400">Insurance</span>.uz
            </h1>
            <p class="text-blue-100 text-lg md:text-xl max-w-2xl mx-auto font-medium opacity-80">
                O'zbekiston sug'urta bozori mutaxassislari uchun interaktiv imtihon platformasi
            </p>
        </div>
    </header>

    <main class="flex-grow container mx-auto px-4 py-12 max-w-4xl -mt-16">
        
        <!-- Start Screen -->
        <div id="start-screen" class="custom-glass rounded-[3rem] shadow-[0_32px_64px_-15px_rgba(0,0,0,0.1)] p-10 md:p-16 text-center relative z-20">
            <div class="max-w-md mx-auto">
                <div class="w-24 h-24 bg-blue-600 text-white rounded-3xl flex items-center justify-center mx-auto mb-10 shadow-2xl transform hover:rotate-6 transition-transform">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
                    </svg>
                </div>
                <h2 class="text-4xl font-extrabold text-slate-800 mb-6 tracking-tight">Imtihonga tayyormisiz?</h2>
                
                <div class="grid grid-cols-1 gap-4 mb-10">
                    <div class="flex items-center p-4 bg-blue-50 rounded-2xl border border-blue-100">
                        <div class="w-10 h-10 bg-blue-600 rounded-full flex items-center justify-center text-white mr-4 shrink-0">1</div>
                        <p class="text-slate-700 text-sm font-semibold text-left">10 ta dolzarb professional savol</p>
                    </div>
                    <div class="flex items-center p-4 bg-indigo-50 rounded-2xl border border-indigo-100">
                        <div class="w-10 h-10 bg-indigo-600 rounded-full flex items-center justify-center text-white mr-4 shrink-0">2</div>
                        <p class="text-slate-700 text-sm font-semibold text-left">Avtomatik o'tish va darhol natija</p>
                    </div>
                </div>

                <button onclick="startQuiz()" class="group relative w-full py-6 bg-blue-600 hover:bg-blue-700 text-white font-black rounded-2xl shadow-2xl shadow-blue-200 transition-all transform hover:-translate-y-1 active:scale-95 text-xl overflow-hidden">
                    <span class="relative z-10">TESTNI BOSHLASH</span>
                    <div class="absolute inset-0 bg-white opacity-0 group-hover:opacity-10 transition-opacity"></div>
                </button>
            </div>
        </div>

        <!-- Quiz Screen -->
        <div id="quiz-screen" class="hidden">
            <div class="custom-glass rounded-[3rem] shadow-[0_32px_64px_-15px_rgba(0,0,0,0.1)] p-8 md:p-12 border border-white">
                <div class="flex flex-col md:flex-row justify-between items-center mb-12 gap-6">
                    <div class="flex items-center space-x-4">
                        <div class="px-5 py-2 bg-blue-600 text-white rounded-2xl font-black text-sm tracking-tighter" id="progress-text">
                            SAVOL: 1 / 10
                        </div>
                        <span class="h-1.5 w-1.5 rounded-full bg-slate-300"></span>
                        <span class="text-slate-400 text-xs font-extrabold uppercase tracking-widest">Hayot sug'urtasi</span>
                    </div>
                    <div class="w-full md:w-64">
                        <div class="h-3 bg-slate-100 rounded-full overflow-hidden p-0.5 border border-slate-200">
                            <div id="progress-bar" class="h-full bg-blue-600 rounded-full transition-all duration-500 progress-glow" style="width: 10%"></div>
                        </div>
                    </div>
                </div>

                <div class="min-h-[140px] mb-12">
                    <h3 id="question-text" class="text-2xl md:text-4xl font-extrabold text-slate-800 leading-[1.2] tracking-tight">
                        Yuklanmoqda...
                    </h3>
                </div>

                <div id="options-container" class="grid grid-cols-1 gap-4">
                    <!-- Options injected here -->
                </div>
            </div>
        </div>

        <!-- Result Screen -->
        <div id="result-screen" class="hidden">
            <div class="text-center custom-glass rounded-[3rem] shadow-[0_32px_64px_-15px_rgba(0,0,0,0.1)] p-12 md:p-20 border border-white mb-10">
                <div id="result-icon-container" class="mb-8"></div>
                <h2 class="text-5xl font-black text-slate-800 mb-4 tracking-tighter">Natijangiz</h2>
                
                <div class="relative w-64 h-64 mx-auto my-12">
                    <svg class="w-full h-full transform -rotate-90" viewBox="0 0 36 36">
                        <circle cx="18" cy="18" r="16" fill="none" class="text-slate-100" stroke-width="2.5" stroke="currentColor"></circle>
                        <circle id="score-stroke" cx="18" cy="18" r="16" fill="none" class="text-blue-600" stroke-width="2.5" stroke-dasharray="0, 100" stroke-linecap="round" stroke="currentColor" style="transition: stroke-dasharray 2s cubic-bezier(0.4, 0, 0.2, 1)"></circle>
                    </svg>
                    <div class="absolute inset-0 flex flex-col items-center justify-center">
                        <span id="final-score" class="text-7xl font-black text-slate-800 tracking-tighter">0/10</span>
                        <span id="score-percent" class="text-xs font-black text-blue-600 mt-2 tracking-[0.3em] uppercase">0% MUVAFFAQIYAT</span>
                    </div>
                </div>

                <p id="result-message" class="text-xl text-slate-600 mb-12 font-medium max-w-md mx-auto leading-relaxed"></p>
                
                <div class="flex flex-col sm:flex-row gap-4 justify-center">
                    <button onclick="resetQuiz()" class="px-10 py-5 bg-slate-900 hover:bg-black text-white font-bold rounded-2xl transition-all shadow-xl hover:-translate-y-1">
                        Qayta topshirish
                    </button>
                    <button onclick="document.getElementById('detailed-report').scrollIntoView({behavior:'smooth'})" class="px-10 py-5 bg-white border-2 border-slate-200 text-slate-700 font-bold rounded-2xl hover:bg-slate-50 transition-all">
                        Xatolar tahlili
                    </button>
                </div>
            </div>

            <!-- Detailed Report Section -->
            <div id="detailed-report" class="bg-white rounded-[2.5rem] shadow-2xl p-8 md:p-12 border border-slate-100">
                <h3 class="text-3xl font-black text-slate-800 mb-10 flex items-center tracking-tight">
                    <span class="w-12 h-12 bg-blue-100 text-blue-600 rounded-2xl flex items-center justify-center mr-4 shadow-inner">📋</span>
                    Savollar tahlili
                </h3>
                <div id="report-container" class="space-y-8">
                    <!-- Report items injected here -->
                </div>
            </div>
        </div>
    </main>

    <footer class="py-12 text-center text-slate-400 text-sm mt-auto">
        <div class="flex justify-center space-x-2 mb-4">
            <span class="w-8 h-1 bg-slate-200 rounded-full"></span>
            <span class="w-8 h-1 bg-blue-600 rounded-full"></span>
            <span class="w-8 h-1 bg-slate-200 rounded-full"></span>
        </div>
        <p class="font-black text-slate-600 uppercase tracking-[0.4em] mb-2 text-xs">HayotInsurance.uz</p>
        <p>© 2026 Professional Sug'urta Platformasi</p>
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
                q: "Sug'urta agenti bir vaqtning o'zida nechta hayot sug'urtasi kompaniyasi bilan shartnoma tuzishi mumkin?",
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
                options: ["Sug'urta polisi sotuvchisi", "Xavf hamda tariflarni hisoblovchi mutaxassis", "Kompaniya advokati", "Mijozlarni qabul qiluvchi xodim"],
                correct: 1
            },
            {
                q: "Hayot sug'urtasida 'Surrender Value' (Viykupnaya summa) nima?",
                options: ["Agentning mukofot puli", "Bekor qilinganda mijozga qaytariladigan to'plangan mablag' qismi", "Soliq jarimasi", "Kompaniya sof foydasi"],
                correct: 1
            },
            {
                q: "JShDS bo'yicha imtiyoz qo'llanilishi uchun hayot sug'urtasi shartnomasi qancha muddatga tuzilgan bo'lishi shart?",
                options: ["Kamida 6 oy", "Kamida 12 oy", "Kamida 3 yil", "Muddat ahamiyatga ega emas"],
                correct: 1
            }
        ];

        let currentQuestions = [];
        let currentQuestionIndex = 0;
        let score = 0;
        let userAnswers = [];
        let canAnswer = true;

        function startQuiz() {
            currentQuestions = [...allQuestions].sort(() => 0.5 - Math.random());
            currentQuestionIndex = 0;
            score = 0;
            userAnswers = [];
            canAnswer = true;
            
            document.getElementById('start-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            
            showQuestion();
        }

        function showQuestion() {
            const question = currentQuestions[currentQuestionIndex];
            canAnswer = true;
            
            document.getElementById('progress-text').innerText = `SAVOL: ${currentQuestionIndex + 1} / 10`;
            document.getElementById('progress-bar').style.width = `${(currentQuestionIndex + 1) * 10}%`;
            document.getElementById('question-text').innerText = question.q;
            
            const optionsContainer = document.getElementById('options-container');
            optionsContainer.innerHTML = '';
            
            question.options.forEach((opt, index) => {
                const btn = document.createElement('button');
                btn.className = "option-btn w-full text-left p-6 border-2 border-slate-100 rounded-3xl flex items-center group relative bg-white";
                btn.innerHTML = `
                    <div class="w-12 h-12 rounded-2xl bg-slate-50 text-slate-400 flex items-center justify-center mr-6 font-black text-lg border border-slate-100 transition-colors group-hover:bg-blue-50 group-hover:text-blue-600">
                        ${String.fromCharCode(65 + index)}
                    </div>
                    <span class="text-slate-700 font-bold text-lg md:text-xl flex-1">${opt}</span>
                    <div class="status-icon opacity-0 ml-4 transition-opacity"></div>
                `;
                btn.onclick = () => selectAndNext(index, btn);
                optionsContainer.appendChild(btn);
            });
        }

        function selectAndNext(index, element) {
            if (!canAnswer) return;
            canAnswer = false;

            const isCorrect = index === currentQuestions[currentQuestionIndex].correct;
            userAnswers.push(index);

            if (isCorrect) {
                score++;
                element.classList.add('correct-anim');
                element.querySelector('.status-icon').innerHTML = '<span class="text-2xl text-green-600">✓</span>';
            } else {
                element.classList.add('wrong-anim');
                element.querySelector('.status-icon').innerHTML = '<span class="text-2xl text-red-600">✕</span>';
                
                // To'g'ri javobni ham ko'rsatish
                const buttons = document.getElementById('options-container').children;
                const correctIdx = currentQuestions[currentQuestionIndex].correct;
                buttons[correctIdx].classList.add('correct-anim');
            }
            
            element.querySelector('.status-icon').classList.remove('opacity-0');

            // Avtomatik keyingi savolga o'tish (1 soniya kechikish bilan)
            setTimeout(() => {
                currentQuestionIndex++;
                if (currentQuestionIndex < currentQuestions.length) {
                    showQuestion();
                } else {
                    showResult();
                }
            }, 1000);
        }

        function showResult() {
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.remove('hidden');
            
            const finalScoreEl = document.getElementById('final-score');
            const percent = score * 10;
            
            finalScoreEl.innerText = `${score}/10`;
            document.getElementById('score-percent').innerText = `${percent}% MUVAFFAQIYAT`;
            
            setTimeout(() => {
                document.getElementById('score-stroke').setAttribute('stroke-dasharray', `${percent}, 100`);
            }, 200);
            
            let message = "";
            let icon = "";
            if (score === 10) {
                message = "Mukammal! Siz sohaning yetuk mutaxassisi ekanligingizni isbotladingiz.";
                icon = "🏆";
            } else if (score >= 7) {
                message = "Ajoyib natija! Professional malakangiz yuqori darajada.";
                icon = "🌟";
            } else {
                message = "Yomon emas, lekin bilimingizni yanada mustahkamlash uchun qonun hujjatlarini qayta ko'rib chiqing.";
                icon = "📚";
            }
            
            document.getElementById('result-message').innerText = message;
            document.getElementById('result-icon-container').innerHTML = `<span class="text-7xl">${icon}</span>`;

            generateReport();
        }

        function generateReport() {
            const reportContainer = document.getElementById('report-container');
            reportContainer.innerHTML = '';

            currentQuestions.forEach((question, index) => {
                const userChoice = userAnswers[index];
                const isCorrect = userChoice === question.correct;

                const item = document.createElement('div');
                item.className = `p-8 rounded-[2rem] border-2 transition-all hover:shadow-lg ${isCorrect ? 'bg-green-50 border-green-100' : 'bg-red-50 border-red-100'}`;
                
                item.innerHTML = `
                    <div class="flex flex-wrap items-center gap-4 mb-6">
                        <span class="text-xs font-black px-4 py-1.5 rounded-full ${isCorrect ? 'bg-green-600 text-white' : 'bg-red-600 text-white'} uppercase">
                            ${isCorrect ? 'To\'g\'ri' : 'Xato'}
                        </span>
                        <span class="text-slate-400 font-bold text-sm tracking-widest uppercase">Savol #${index + 1}</span>
                    </div>
                    <p class="text-slate-800 font-extrabold text-xl mb-6 leading-tight">${question.q}</p>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div class="p-4 rounded-2xl bg-white border border-slate-100 shadow-sm">
                            <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Sizning javobingiz</p>
                            <p class="font-bold ${isCorrect ? 'text-green-600' : 'text-red-600'}">${question.options[userChoice]}</p>
                        </div>
                        ${!isCorrect ? `
                        <div class="p-4 rounded-2xl bg-white border border-slate-100 shadow-sm">
                            <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">To'g'ri javob</p>
                            <p class="font-bold text-green-600">${question.options[question.correct]}</p>
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
