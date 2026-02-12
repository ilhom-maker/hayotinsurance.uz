import React, { useState, useEffect } from 'react';
import { initializeApp } from 'firebase/app';
import { getAuth, signInAnonymously, onAuthStateChanged, signInWithCustomToken } from 'firebase/auth';
import { getFirestore, collection, addDoc, onSnapshot, query, doc, serverTimestamp } from 'firebase/firestore';
import { 
  CheckCircle, XCircle, ChevronRight, RotateCcw, 
  Award, ShieldCheck, ListChecks, ArrowLeft, 
  HeartPulse, Zap, Target, Star, User, Settings, Database, Trash2
} from 'lucide-react';

// Firebase configuration
const firebaseConfig = JSON.parse(__firebase_config);
const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const appId = typeof __app_id !== 'undefined' ? __app_id : 'insurance-quiz-app';

const App = () => {
  const [user, setUser] = useState(null);
  const [userName, setUserName] = useState('');
  const [tempName, setTempName] = useState('');
  const [currentStep, setCurrentStep] = useState('welcome');
  const [shuffledQuestions, setShuffledQuestions] = useState([]);
  const [currentQuestionIdx, setCurrentQuestionIdx] = useState(0);
  const [score, setScore] = useState(0);
  const [showFeedback, setShowFeedback] = useState(null); 
  const [selectedAnswer, setSelectedAnswer] = useState(null);
  const [userAnswers, setUserAnswers] = useState([]);
  
  // Admin states
  const [adminPass, setAdminPass] = useState('');
  const [allReports, setAllReports] = useState([]);
  const [adminError, setAdminError] = useState('');

  const rawQuestions = [
    {
      question: "Hayotni sug'urta qilishda 'Annuitet' nima?",
      options: ["Bir martalik to'lov", "Muntazam ravishda to'lanadigan sug'urta rentasi", "Sug'urta jarimasi", "Polisni bekor qilish to'lovi"],
      correct: "Muntazam ravishda to'lanadigan sug'urta rentasi"
    },
    {
      question: "Hayotni uzoq muddatli sug'urta qilishda 'Surrender value' (qaytarish summasi) nima?",
      options: ["Sug'urta mukofoti miqdori", "Shartnoma muddatidan oldin bekor qilinganda to'lanadigan mablag'", "Vafot etganda beriladigan summa", "Agentning komissiyasi"],
      correct: "Shartnoma muddatidan oldin bekor qilinganda to'lanadigan mablag'"
    },
    {
      question: "Hayotni sug'urta qilish shartnomasida 'Benefitsiar' kim?",
      options: ["Sug'urta agenti", "Sug'urta kompaniyasi direktori", "Sug'urta tovoni olishga haqli bo'lgan tayinlangan shaxs", "Davlat soliq inspektori"],
      correct: "Sug'urta tovoni olishga haqli bo'lgan tayinlangan shaxs"
    },
    {
      question: "Qaysi sug'urta turi ham himoya, ham jamg'arma funksiyasini bajaradi?",
      options: ["Avtotransport sug'urtasi", "Hayotni jamg'arib boriladigan sug'urta qilish", "Yong'indan sug'urta", "Sayohat sug'urtasi"],
      correct: "Hayotni jamg'arib boriladigan sug'urta qilish"
    },
    {
      question: "Hayotni sug'urta qilishda 'Anderrayting' jarayoni nima uchun kerak?",
      options: ["Reklama qilish uchun", "Mijozning sog'lig'i va xatarlarini baholash uchun", "Ofisni bezash uchun", "Polisni chop etish uchun"],
      correct: "Mijozning sog'lig'i va xatarlarini baholash uchun"
    },
    {
      question: "Hayotni sug'urta qilishda 'Kritik kasalliklar' qoplamasi nimani anglatadi?",
      options: ["Oddiy shamollashni davolash", "Og'ir kasalliklar (saraton, insult va b.) aniqlanganda to'lanadigan summa", "Faqat tish davolash", "Sport bilan shug'ullanish xarajatlari"],
      correct: "Og'ir kasalliklar (saraton, insult va b.) aniqlanganda to'lanadigan summa"
    },
    {
      question: "Guruhli hayot sug'urtasi odatda kimlar uchun rasmiylashtiriladi?",
      options: ["Faqat oila a'zolari uchun", "Kompaniya xodimlari uchun ish beruvchi tomonidan", "Faqat talabalar uchun", "Faqat pensionerlar uchun"],
      correct: "Kompaniya xodimlari uchun ish beruvchi tomonidan"
    },
    {
      question: "Hayotni sug'urta qilishda 'Grace period' (imtiyozli davr) nima?",
      options: ["Tushlik vaqti", "Sug'urta mukofotini kechikib to'lashga ruxsat berilgan muddat", "Shartnoma tugash muddati", "Dam olish kunlari"],
      correct: "Sug'urta mukofoti kechikib to'lashga ruxsat berilgan muddat"
    },
    {
      question: "Hayotni sug'urta qilish polislari bo'yicha daromad solig'i imtiyozi O'zbekistonda mavjudmi?",
      options: ["Yo'q, mavjud emas", "Ha, ish haqidan to'lanadigan sug'urta mukofotlari soliqdan ozod etiladi", "Faqat chet el fuqarolari uchun", "Faqat pensiya yoshidagilar uchun"],
      correct: "Ha, ish haqidan to'lanadigan sug'urta mukofotlari soliqdan ozod etiladi"
    },
    {
      question: "Unit-Linked sug'urta mahsuloti nimasi bilan ajralib turadi?",
      options: ["Faqat vafotni qoplaydi", "Sug'urta mukofotining bir qismi investitsiya qilinadi", "Soliq imtiyozi yo'q", "Faqat 1 yilga tuziladi"],
      correct: "Sug'urta mukofotining bir qismi investitsiya qilinadi"
    }
  ];

  // RULE 3: Auth Before Queries
  useEffect(() => {
    const initAuth = async () => {
      try {
        if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
          await signInWithCustomToken(auth, __initial_auth_token);
        } else {
          await signInAnonymously(auth);
        }
      } catch (err) {
        console.error("Auth initialization error:", err);
      }
    };
    initAuth();
    const unsubscribe = onAuthStateChanged(auth, (user) => {
      setUser(user);
    });
    return () => unsubscribe();
  }, []);

  // Admin: Fetch all reports (RULE 1 & 2)
  useEffect(() => {
    if (!user || currentStep !== 'admin_panel') return;
    
    // Path: /artifacts/{appId}/public/data/{collectionName}
    const reportsCol = collection(db, 'artifacts', appId, 'public', 'data', 'reports');
    
    const unsubscribe = onSnapshot(reportsCol, (snapshot) => {
      const data = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
      // Sort in JS to avoid index requirement (RULE 2)
      setAllReports(data.sort((a, b) => (b.timestamp?.seconds || 0) - (a.timestamp?.seconds || 0)));
    }, (error) => {
      console.error("Firestore listening error:", error);
    });
    
    return () => unsubscribe();
  }, [user, currentStep]);

  const saveReportToCloud = async (finalScore, answers) => {
    if (!user) {
      console.error("Cannot save: User not authenticated");
      return;
    }
    
    try {
      // RULE 1: Strict Paths
      const reportsCol = collection(db, 'artifacts', appId, 'public', 'data', 'reports');
      await addDoc(reportsCol, {
        userName: userName || 'Anonymous',
        score: finalScore,
        answers: answers,
        timestamp: serverTimestamp(),
        userId: user.uid
      });
    } catch (err) {
      console.error("Error saving report to cloud:", err);
    }
  };

  const shuffleArray = (array) => {
    return array
      .map((value) => ({ value, sort: Math.random() }))
      .sort((a, b) => a.sort - b.sort)
      .map(({ value }) => value);
  };

  const handleWelcomeSubmit = (e) => {
    e.preventDefault();
    const name = tempName.trim();
    const isValidName = name.length >= 3 && /^[a-zA-Zа-яА-ЯёЁқҚғҒҳҲўЎ\s]+$/.test(name);
    
    if (isValidName) {
      setUserName(name);
      setCurrentStep('start');
    }
  };

  const initQuiz = () => {
    const shuffled = shuffleArray(rawQuestions).map(q => ({
      ...q,
      options: shuffleArray(q.options)
    }));
    setShuffledQuestions(shuffled);
    setCurrentQuestionIdx(0);
    setScore(0);
    setUserAnswers([]);
    setCurrentStep('quiz');
    setShowFeedback(null);
    setSelectedAnswer(null);
  };

  const handleAnswer = (option) => {
    if (showFeedback) return;
    setSelectedAnswer(option);
    const isCorrect = option === shuffledQuestions[currentQuestionIdx].correct;
    
    const currentQuestionAnswers = {
      question: shuffledQuestions[currentQuestionIdx].question,
      selected: option,
      correct: shuffledQuestions[currentQuestionIdx].correct,
      isCorrect: isCorrect
    };
    
    const newAnswers = [...userAnswers, currentQuestionAnswers];
    const newScore = isCorrect ? score + 1 : score;
    
    if (isCorrect) {
      setScore(newScore);
      setShowFeedback('correct');
    } else {
      setShowFeedback('wrong');
    }

    setUserAnswers(newAnswers);

    setTimeout(() => {
      setShowFeedback(null);
      setSelectedAnswer(null);
      if (currentQuestionIdx < shuffledQuestions.length - 1) {
        setCurrentQuestionIdx(currentQuestionIdx + 1);
      } else {
        saveReportToCloud(newScore, newAnswers);
        setCurrentStep('result');
      }
    }, 1000);
  };

  const handleAdminLogin = (e) => {
    e.preventDefault();
    if (adminPass === '7777') {
      setAdminError('');
      setCurrentStep('admin_panel');
    } else {
      setAdminError('Xato kod!');
    }
  };

  // NEW RED PANDA DESIGN
  const RedPandaAvatar = ({ mood, size = "w-36 h-36" }) => (
    <div className={`relative ${size} mx-auto transition-all duration-500 transform ${mood === 'correct' ? 'scale-110' : mood === 'wrong' ? 'shake-animation' : 'hover:scale-105'}`}>
      <svg viewBox="0 0 200 200" className="w-full h-full drop-shadow-2xl">
        {/* Ears */}
        <path d="M45 55 L15 15 Q40 5 70 40 Z" fill="#E65100" />
        <path d="M155 55 L185 15 Q160 5 130 40 Z" fill="#E65100" />
        <path d="M50 50 L30 25 Q45 20 60 40 Z" fill="white" />
        <path d="M150 50 L170 25 Q155 20 140 40 Z" fill="white" />
        
        {/* Face Base */}
        <circle cx="100" cy="100" r="80" fill="#FF7043" />
        
        {/* White Markings Around Eyes */}
        <ellipse cx="60" cy="95" rx="35" ry="40" fill="white" opacity="0.9" />
        <ellipse cx="140" cy="95" rx="35" ry="40" fill="white" opacity="0.9" />
        
        {/* Brown Stripes on Cheeks */}
        <path d="M30 110 Q45 105 50 125" fill="none" stroke="#5D4037" strokeWidth="8" strokeLinecap="round" opacity="0.4" />
        <path d="M170 110 Q155 105 150 125" fill="none" stroke="#5D4037" strokeWidth="8" strokeLinecap="round" opacity="0.4" />
        
        {/* Eyes */}
        <g className={mood === 'wrong' ? 'translate-y-1' : ''}>
           <circle cx="65" cy="95" r="10" fill="#212121" />
           <circle cx="135" cy="95" r="10" fill="#212121" />
           <circle cx="62" cy="92" r="3" fill="white" />
           <circle cx="132" cy="92" r="3" fill="white" />
        </g>
        
        {/* "Eyebrows" / Red Markings */}
        <path d="M50 75 Q65 65 80 75" fill="none" stroke="#D84315" strokeWidth="6" strokeLinecap="round" />
        <path d="M120 75 Q135 65 150 75" fill="none" stroke="#D84315" strokeWidth="6" strokeLinecap="round" />
        
        {/* Muzzle */}
        <ellipse cx="100" cy="130" rx="30" ry="25" fill="white" />
        <path d="M90 125 Q100 135 110 125" fill="none" stroke="#212121" strokeWidth="3" />
        <rect x="94" y="115" width="12" height="7" rx="4" fill="#212121" />
        
        {/* Mouth Response */}
        {mood === 'correct' ? (
           <path d="M90 140 Q100 155 110 140" fill="none" stroke="#FF5252" strokeWidth="4" strokeLinecap="round" />
        ) : mood === 'wrong' ? (
           <path d="M92 145 Q100 140 108 145" fill="none" stroke="#212121" strokeWidth="2" strokeLinecap="round" />
        ) : (
           <path d="M95 142 Q100 145 105 142" fill="none" stroke="#212121" strokeWidth="2" strokeLinecap="round" />
        )}

        {/* Status Indicators */}
        {mood === 'correct' && (
          <g transform="translate(150, 40)">
            <circle r="25" fill="#4CAF50" stroke="white" strokeWidth="3" />
            <path d="M-8 2 L-2 8 L10 -4" fill="none" stroke="white" strokeWidth="5" strokeLinecap="round" />
          </g>
        )}
        {mood === 'wrong' && (
          <g transform="translate(150, 40)">
            <circle r="25" fill="#F44336" stroke="white" strokeWidth="3" />
            <path d="M-7 -7 L7 7 M7 -7 L-7 7" fill="none" stroke="white" strokeWidth="5" strokeLinecap="round" />
          </g>
        )}
      </svg>
    </div>
  );

  return (
    <div className="min-h-screen bg-[#F8FAFC] flex items-center justify-center p-4 font-sans text-slate-800 overflow-hidden relative">
      <div className="absolute top-[-10%] left-[-10%] w-[60%] h-[60%] bg-red-100/50 blur-[120px] rounded-full"></div>
      <div className="absolute bottom-[-10%] right-[-10%] w-[60%] h-[60%] bg-blue-100/50 blur-[120px] rounded-full"></div>

      {/* Admin Trigger */}
      {currentStep === 'welcome' && (
        <button 
          onClick={() => setCurrentStep('admin_auth')}
          className="absolute top-6 right-6 p-4 bg-white/50 hover:bg-white rounded-2xl transition-all shadow-sm flex items-center gap-2 font-bold text-slate-500 hover:text-red-600 z-50"
        >
          <Settings size={18} /> Admin
        </button>
      )}

      <div className={`w-full transition-all duration-700 ease-out bg-white/90 backdrop-blur-2xl rounded-[3rem] shadow-[0_20px_70px_-15px_rgba(0,0,0,0.1)] border border-white overflow-hidden flex flex-col ${['review', 'admin_panel'].includes(currentStep) ? 'max-w-6xl min-h-[85vh]' : 'max-w-4xl min-h-[65vh]'}`}>
        
        {/* ADMIN AUTH SCREEN */}
        {currentStep === 'admin_auth' && (
          <div className="p-8 text-center flex flex-col items-center justify-center flex-1">
            <ShieldCheck size={48} className="text-red-600 mb-4" />
            <h2 className="text-2xl font-black mb-6 uppercase">ADMIN KIRISH</h2>
            <form onSubmit={handleAdminLogin} className="w-full max-w-xs space-y-4">
              <input 
                type="password" 
                value={adminPass}
                onChange={(e) => setAdminPass(e.target.value)}
                placeholder="Maxfiy kod..."
                className="w-full bg-white border-2 border-slate-100 rounded-2xl py-4 px-6 text-center font-bold text-xl focus:border-red-500 outline-none transition-all"
                autoFocus
              />
              {adminError && <p className="text-red-500 font-bold text-sm">{adminError}</p>}
              <div className="grid grid-cols-2 gap-3">
                <button type="button" onClick={() => setCurrentStep('welcome')} className="bg-slate-100 py-4 rounded-2xl font-bold">Bekor qilish</button>
                <button type="submit" className="bg-slate-900 text-white py-4 rounded-2xl font-bold">Kirish</button>
              </div>
            </form>
          </div>
        )}

        {/* ADMIN PANEL SCREEN */}
        {currentStep === 'admin_panel' && (
          <div className="p-8 flex flex-col flex-1 overflow-hidden">
            <div className="flex items-center justify-between mb-8">
              <div className="flex items-center gap-4">
                <Database className="text-red-600" size={32} />
                <h1 className="text-3xl font-black uppercase tracking-tighter">Foydalanuvchilar Hisoboti</h1>
              </div>
              <button onClick={() => setCurrentStep('welcome')} className="bg-slate-900 text-white px-6 py-3 rounded-2xl font-bold hover:bg-red-600 transition-colors">Chiqish</button>
            </div>

            <div className="flex-1 overflow-y-auto pr-4 custom-scrollbar">
              <table className="w-full border-separate border-spacing-y-4">
                <thead>
                  <tr className="text-left text-slate-400 text-xs uppercase tracking-widest font-black">
                    <th className="px-6 pb-2">Ism</th>
                    <th className="px-6 pb-2">Natija</th>
                    <th className="px-6 pb-2">Vaqt</th>
                    <th className="px-6 pb-2">Tafsilotlar</th>
                  </tr>
                </thead>
                <tbody>
                  {allReports.map((report) => (
                    <tr key={report.id} className="bg-white rounded-3xl shadow-sm border border-slate-50 group hover:shadow-md transition-all">
                      <td className="px-6 py-5 font-black text-slate-900">{report.userName}</td>
                      <td className="px-6 py-5">
                        <span className={`px-4 py-1.5 rounded-full font-black text-xs ${report.score >= 7 ? 'bg-emerald-100 text-emerald-600' : 'bg-red-100 text-red-600'}`}>
                          {report.score} / 10
                        </span>
                      </td>
                      <td className="px-6 py-5 text-sm text-slate-400 font-medium">
                        {report.timestamp?.toDate ? report.timestamp.toDate().toLocaleString('uz-UZ') : 'Hozir...'}
                      </td>
                      <td className="px-6 py-5">
                        <div className="flex flex-wrap gap-1">
                          {report.answers?.map((a, i) => (
                            <div key={i} title={a.question} className={`w-3 h-3 rounded-full ${a.isCorrect ? 'bg-emerald-400' : 'bg-red-400'}`}></div>
                          ))}
                        </div>
                      </td>
                    </tr>
                  ))}
                  {allReports.length === 0 && (
                    <tr>
                      <td colSpan="4" className="text-center py-20 text-slate-400 font-bold">Hozircha ma'lumotlar yo'q</td>
                    </tr>
                  )}
                </tbody>
              </table>
            </div>
          </div>
        )}

        {/* WELCOME SCREEN (NAME INPUT) */}
        {currentStep === 'welcome' && (
          <div className="p-8 text-center relative flex flex-col items-center justify-center flex-1">
             <div className="mb-6">
              <RedPandaAvatar size="w-32 h-32" />
            </div>
            <div className="bg-red-50 px-6 py-4 rounded-3xl border border-red-100 mb-8 relative">
                <div className="absolute -top-3 left-1/2 -translate-x-1/2 w-4 h-4 bg-red-50 border-l border-t border-red-100 rotate-45"></div>
                <p className="text-xl font-bold text-slate-800">Assalomu alaykum! Men Alfa ismli qizil pandaman. Ismingiz nima?</p>
            </div>
            <form onSubmit={handleWelcomeSubmit} className="w-full max-w-sm flex flex-col gap-4">
              <div className="relative">
                <User className="absolute left-5 top-1/2 -translate-y-1/2 text-slate-400" size={20} />
                <input 
                  type="text" 
                  value={tempName}
                  onChange={(e) => setTempName(e.target.value)}
                  placeholder="Ismingizni kiriting..."
                  className="w-full bg-white border-2 border-slate-100 rounded-2xl py-5 pl-14 pr-6 font-bold text-lg focus:border-red-500 outline-none transition-all shadow-inner"
                  autoFocus
                />
              </div>
              <button 
                type="submit"
                disabled={!tempName.trim() || tempName.trim().length < 3 || /\d/.test(tempName)}
                className="bg-slate-900 text-white font-black py-5 rounded-2xl transition-all active:scale-95 shadow-xl hover:bg-red-600 disabled:opacity-50 disabled:bg-slate-300 tracking-widest uppercase text-lg"
              >
                Tanishganimdan xursandman
              </button>
            </form>
          </div>
        )}

        {/* START SCREEN */}
        {currentStep === 'start' && (
          <div className="p-8 text-center relative flex flex-col items-center justify-center flex-1">
            <div className="inline-flex p-4 rounded-[2rem] bg-gradient-to-br from-red-500 to-orange-600 shadow-[0_15px_35px_rgba(239,68,68,0.25)] mb-6">
              <HeartPulse size={40} className="text-white" />
            </div>
            <h1 className="text-4xl font-black text-slate-900 mb-4 uppercase tracking-tighter">
              Sug'urta <span className="text-red-600">Testi</span>
            </h1>
            <RedPandaAvatar size="w-32 h-32" />
            <p className="text-slate-500 mt-6 mb-8 text-lg max-w-lg mx-auto leading-relaxed font-medium">
              Xush kelibsiz, <span className="text-red-600 font-bold">{userName}</span>! Men Alfa ismli qizil pandaman. Hayotni sug'urta qilish bo'yicha professional bilimlaringizni sinab ko'ring.
            </p>
            <button 
              onClick={initQuiz}
              className="group relative w-full max-w-sm bg-slate-900 text-white font-black py-5 rounded-[1.5rem] transition-all overflow-hidden flex items-center justify-center gap-4 active:scale-95 shadow-xl hover:bg-red-600"
            >
              <span className="relative z-10 text-xl tracking-widest uppercase">Boshlash</span>
              <Zap size={22} className="relative z-10 fill-yellow-400 text-yellow-400" />
            </button>
          </div>
        )}

        {/* QUIZ SCREEN */}
        {currentStep === 'quiz' && (
          <div className="p-8 flex flex-col flex-1">
            <div className="flex justify-between items-center mb-6 bg-white shadow-sm border border-slate-100 p-4 rounded-3xl">
              <div className="flex items-center gap-4">
                <div className="h-12 w-12 rounded-2xl bg-slate-900 flex items-center justify-center text-white font-black text-xl shadow-lg">
                  {currentQuestionIdx + 1}
                </div>
                <div>
                  <h3 className="text-[10px] font-black text-slate-400 uppercase tracking-widest leading-none mb-1">{userName} uchun savol</h3>
                  <div className="flex items-center gap-1.5">
                    {[...Array(10)].map((_, i) => (
                      <div 
                        key={i} 
                        className={`h-1 w-4 rounded-full transition-all ${i <= currentQuestionIdx ? 'bg-red-600 w-8' : 'bg-slate-200'}`}
                      />
                    ))}
                  </div>
                </div>
              </div>
              <div className="flex items-center gap-8">
                <RedPandaAvatar mood={showFeedback} size="w-16 h-16" />
                <div className="text-right">
                  <span className="block text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">To'g'ri</span>
                  <span className="text-xl font-black text-emerald-500">{score}</span>
                </div>
              </div>
            </div>

            <div className="relative mb-6 p-8 bg-gradient-to-br from-white to-slate-50 rounded-[2.5rem] border border-slate-100 shadow-[0_10px_30px_-5px_rgba(0,0,0,0.03)] overflow-hidden flex-1 flex items-center">
               <div className="absolute top-0 left-0 w-2 h-full bg-red-600"></div>
               <h2 className="text-2xl md:text-3xl font-black text-slate-900 leading-tight">
                {shuffledQuestions[currentQuestionIdx]?.question}
              </h2>
            </div>

            <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mb-2">
              {shuffledQuestions[currentQuestionIdx]?.options.map((option, idx) => {
                const labels = ['A', 'B', 'C', 'D'];
                let stateClass = "bg-white border-slate-200 hover:border-red-400 hover:shadow-md hover:-translate-y-1";
                let labelBg = "bg-slate-100 text-slate-600";
                
                if (showFeedback) {
                  if (option === shuffledQuestions[currentQuestionIdx].correct) {
                    stateClass = "bg-emerald-50 border-emerald-500 text-emerald-700 ring-4 ring-emerald-50";
                    labelBg = "bg-emerald-500 text-white";
                  } else if (option === selectedAnswer) {
                    stateClass = "bg-red-50 border-red-500 text-red-700 ring-4 ring-red-50";
                    labelBg = "bg-red-500 text-white";
                  } else {
                    stateClass = "opacity-40 grayscale border-slate-100";
                    labelBg = "bg-slate-50 text-slate-300";
                  }
                }

                return (
                  <button
                    key={idx}
                    disabled={showFeedback}
                    onClick={() => handleAnswer(option)}
                    className={`group relative p-5 rounded-[1.75rem] border-2 transition-all duration-300 flex items-center gap-4 text-left active:scale-95 ${stateClass}`}
                  >
                    <div className={`w-10 h-10 rounded-xl flex items-center justify-center font-black text-base shrink-0 shadow-sm transition-colors ${labelBg}`}>
                      {labels[idx]}
                    </div>
                    <span className="text-base font-bold leading-tight">{option}</span>
                  </button>
                );
              })}
            </div>
          </div>
        )}

        {/* RESULT SCREEN */}
        {currentStep === 'result' && (
          <div className="p-10 text-center flex flex-col flex-1 items-center justify-center">
            <div className="flex flex-row items-center gap-12 mb-8">
              <div className="relative inline-block">
                <div className="absolute inset-0 bg-red-500 blur-[40px] opacity-10 animate-pulse"></div>
                <div className="relative bg-gradient-to-br from-red-500 to-orange-500 p-6 rounded-[2.5rem] shadow-xl border-4 border-white">
                  <Award className="w-14 h-14 text-white" />
                </div>
              </div>
              
              <div className="text-left">
                <h2 className="text-4xl font-black text-slate-900 mb-1 tracking-tighter uppercase">Natija</h2>
                <div className="text-6xl font-black text-red-600 leading-none">{score}<span className="text-2xl text-slate-300">/10</span></div>
              </div>
            </div>
            
            <div className="w-full bg-slate-50 p-8 rounded-[2.5rem] mb-8 border border-slate-100 shadow-inner flex flex-row items-center justify-around gap-8">
              <RedPandaAvatar mood={score >= 7 ? 'correct' : null} size="w-32 h-32" />
              <div className="space-y-2 text-left max-w-md">
                 <p className="text-2xl font-black text-slate-900 tracking-widest uppercase">
                    {userName}, {score === 10 ? "ELITA AGENT" : score >= 7 ? "EKSPERT" : "STAJYOR"}
                 </p>
                 <p className="text-slate-500 font-medium text-base leading-relaxed">
                    {score === 10 ? `Ajoyib natija, ${userName}! Men Alfa, siz sug'urta bo'yicha mastersiz.` : 
                     score >= 7 ? `Juda yaxshi, ${userName}! Bilimingiz professional darajada.` : 
                     `${userName}, bilimlaringizni yanada mustahkamlashingiz kerak.`}
                 </p>
              </div>
            </div>

            <div className="grid grid-cols-2 gap-4 w-full max-w-lg">
              <button 
                onClick={() => setCurrentStep('review')}
                className="bg-white hover:bg-slate-50 text-slate-900 font-black py-5 rounded-[1.25rem] transition-all flex items-center justify-center gap-3 border-2 border-slate-100 shadow-sm"
              >
                <ListChecks size={20} className="text-red-600" /> TAHLIL
              </button>
              <button 
                onClick={initQuiz}
                className="bg-red-600 hover:bg-red-700 text-white font-black py-5 rounded-[1.25rem] transition-all flex items-center justify-center gap-3 shadow-lg shadow-red-200"
              >
                <RotateCcw size={20} /> QAYTA
              </button>
            </div>
          </div>
        )}

        {/* REVIEW SCREEN */}
        {currentStep === 'review' && (
          <div className="p-8 flex flex-col flex-1">
            <div className="flex items-center justify-between mb-8">
              <div className="flex items-center gap-5">
                <button onClick={() => setCurrentStep('result')} className="p-3 bg-slate-50 hover:bg-red-600 hover:text-white rounded-2xl transition-all border border-slate-100 group">
                  <ArrowLeft size={20} />
                </button>
                <h2 className="text-3xl font-black text-slate-900 tracking-tighter uppercase">{userName} tahlili</h2>
              </div>
              <div className="px-5 py-2 bg-red-600 text-white rounded-full font-black text-xs shadow-md tracking-widest">
                {score * 10}% ANIQLIK
              </div>
            </div>

            <div className="space-y-4 flex-1 overflow-y-auto pr-4 custom-scrollbar">
              {userAnswers.map((ans, i) => (
                <div key={i} className={`group p-6 rounded-[2rem] border-2 transition-all duration-300 ${ans.isCorrect ? 'border-emerald-100 bg-emerald-50/30' : 'border-red-100 bg-red-50/30'}`}>
                  <div className="flex items-start gap-4">
                    <div className={`mt-1 w-8 h-8 rounded-xl flex items-center justify-center shrink-0 shadow-sm ${ans.isCorrect ? 'bg-emerald-500 text-white' : 'bg-red-600 text-white'}`}>
                      {ans.isCorrect ? <CheckCircle size={18} /> : <XCircle size={18} />}
                    </div>
                    <div className="flex-1">
                      <p className="font-black text-slate-900 text-lg mb-4 leading-tight">{ans.question}</p>
                      <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div className="bg-white p-4 rounded-2xl border border-slate-100 shadow-sm">
                          <span className="text-[9px] font-black text-slate-400 block mb-1 uppercase tracking-widest">Sizniki</span>
                          <span className={`font-bold text-base ${ans.isCorrect ? 'text-emerald-600' : 'text-red-600'}`}>{ans.selected}</span>
                        </div>
                        {!ans.isCorrect && (
                          <div className="bg-emerald-50 p-4 rounded-2xl border border-emerald-100 shadow-sm">
                            <span className="text-[9px] font-black text-emerald-600 block mb-1 uppercase tracking-widest">To'g'risi</span>
                            <span className="text-emerald-700 font-bold text-base">{ans.correct}</span>
                          </div>
                        )}
                      </div>
                    </div>
                  </div>
                </div>
              ))}
            </div>

            <button 
              onClick={initQuiz}
              className="w-full mt-6 bg-slate-900 hover:bg-red-600 text-white font-black py-5 rounded-[1.25rem] transition-all flex items-center justify-center gap-4 shadow-xl tracking-widest text-lg uppercase"
            >
              <RotateCcw size={22} /> Yangi missiya
            </button>
          </div>
        )}
      </div>
      
      <style>{`
        .custom-scrollbar::-webkit-scrollbar { width: 5px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: #F1F5F9; border-radius: 10px; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #CBD5E1; border-radius: 10px; }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover { background: #EF4444; }
        
        .shake-animation {
          animation: shake 0.5s cubic-bezier(.36,.07,.19,.97) both;
        }
        @keyframes shake {
          10%, 90% { transform: translate3d(-1px, 0, 0); }
          20%, 80% { transform: translate3d(2px, 0, 0); }
          30%, 50%, 70% { transform: translate3d(-4px, 0, 0); }
          40%, 60% { transform: translate3d(4px, 0, 0); }
        }
      `}</style>
    </div>
  );
};

export default App;
