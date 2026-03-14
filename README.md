# fake-news-detector
fake news detection website
import React, { useState, useEffect, useMemo } from 'react';
import { CheckCircle, XCircle, AlertCircle, ArrowLeft, LogOut, ShieldAlert } from 'lucide-react';

// --- 1. DATASET ---
// The provided dataset is embedded here to train our client-side ML model
const csvData = `text,label
Government launches new AI research center in India,REAL
New cybersecurity law passed to protect digital users,REAL
Scientists develop faster quantum computing processor,REAL
Indian Space Research Organisation launches new satellite,REAL
New education policy introduces coding in schools,REAL
Global companies invest in cloud computing infrastructure,REAL
Researchers create energy efficient computer chips,REAL
India hosts international technology summit,REAL
Government announces funding for AI startups,REAL
Scientists improve battery life for laptops,REAL
New software helps detect online fraud,REAL
Cybersecurity experts warn about phishing attacks,REAL
Researchers develop advanced machine learning model,REAL
New smartphone processor improves performance,REAL
Government launches digital literacy campaign,REAL
Major tech companies invest in data centers,REAL
Scientists create new algorithm for faster data search,REAL
Universities introduce artificial intelligence courses,REAL
New programming language gains popularity among developers,REAL
India expands broadband internet access in villages,REAL
Tech companies focus on green computing solutions,REAL
Government plans national cybersecurity strategy,REAL
New computer vision system helps medical diagnosis,REAL
Cloud computing adoption increases worldwide,REAL
Researchers develop AI for climate prediction,REAL
India promotes startup innovation in technology sector,REAL
Cybersecurity training programs launched for students,REAL
Scientists build faster GPU for machine learning,REAL
New software platform helps remote work collaboration,REAL
Tech conference highlights future of robotics,REAL
Government launches digital payment awareness program,REAL
Researchers improve natural language processing models,REAL
New open source tools released for developers,REAL
Scientists develop AI system for traffic management,REAL
India expands 5G technology in major cities,REAL
New cybersecurity tool protects personal data,REAL
Researchers create smart agriculture technology,REAL
Tech firms invest in AI powered healthcare systems,REAL
New database technology improves data storage speed,REAL
Scientists develop computer model for weather forecasting,REAL
Government launches national innovation mission,REAL
New research improves computer graphics rendering,REAL
India collaborates on global AI ethics program,REAL
Researchers create AI chatbot for education support,REAL
Tech startups focus on blockchain technology,REAL
New programming frameworks released for web developers,REAL
Scientists develop energy efficient processors,REAL
Government expands digital governance services,REAL
Cybersecurity experts detect new malware strain,REAL
Researchers develop AI model for language translation,REAL
New computer hardware improves gaming performance,REAL
India promotes semiconductor manufacturing projects,REAL
Scientists research secure communication systems,REAL
Tech companies improve cloud storage security,REAL
New machine learning techniques improve predictions,REAL
Government supports research in robotics engineering,REAL
Scientists develop AI tool for medical imaging,REAL
New coding bootcamps launched for students,REAL
Researchers build advanced neural networks,REAL
Tech firms improve data privacy tools,REAL
India invests in supercomputing infrastructure,REAL
New cybersecurity awareness programs introduced,REAL
Scientists create improved speech recognition AI,REAL
Tech industry expands employment opportunities,REAL
Government supports research in quantum computing,REAL
Researchers develop autonomous vehicle software,REAL
New operating system improves device security,REAL
Tech companies launch smart home devices,REAL
India expands technology parks for startups,REAL
Scientists develop efficient data compression method,REAL
New AI tool improves online learning systems,REAL
Cybersecurity teams prevent major cyber attacks,REAL
Researchers improve robotics automation systems,REAL
New programming tools help beginner coders,REAL
Tech industry focuses on sustainable computing,REAL
India launches satellite internet services,REAL
Scientists develop computer simulation for medicine,REAL
New algorithms improve search engine results,REAL
Researchers study ethical AI development,REAL
Tech companies invest in augmented reality,REAL
Government promotes research in digital innovation,REAL
Scientists develop advanced data analytics tools,REAL
New machine learning models improve accuracy,REAL
India launches new space technology mission,REAL
Researchers improve cybersecurity monitoring systems,REAL
Tech startups develop AI healthcare applications,REAL
Scientists create new neural network architecture,REAL
New software platform supports online education,REAL
Cybersecurity experts detect global hacking attempts,REAL
Researchers develop efficient big data processing,REAL
India expands research in computer science education,REAL
Tech firms improve artificial intelligence assistants,REAL
Scientists build faster computer memory technology,REAL
New digital identity systems improve security,REAL
Researchers develop improved recommendation systems,REAL
Tech companies improve network infrastructure,REAL
Government funds research in AI ethics,REAL
Scientists improve computer chip manufacturing process,REAL
New cloud computing tools help businesses scale,REAL
Researchers develop advanced AI research platforms,REAL
Scientists confirm computers will replace all humans next year,FAKE
Government bans internet permanently across the world,FAKE
Researchers create laptop that runs without electricity forever,FAKE
Aliens send secret coding language to earth programmers,FAKE
New virus spreads through computer screens physically,FAKE
Scientists discover keyboard typing increases height,FAKE
Government plans to delete entire internet tomorrow,FAKE
Researchers invent invisible computers made of air,FAKE
AI system becomes president of a country overnight,FAKE
New smartphone charges using moonlight only,FAKE
Scientists confirm humans will upload brains to USB drives,FAKE
Computer virus can now escape screen and attack people,FAKE
Internet will stop working after 2030 worldwide,FAKE
Researchers develop telepathic programming language,FAKE
Government plans to replace teachers with robots next week,FAKE
New chip gives humans super intelligence instantly,FAKE
Scientists discover WiFi signals cure diseases,FAKE
Laptop batteries will never need charging again magically,FAKE
Hackers steal data through television antennas,FAKE
New app allows users to time travel digitally,FAKE
Scientists say coding daily makes people immortal,FAKE
Government plans to control thoughts using computers,FAKE
Aliens secretly control global internet infrastructure,FAKE
Quantum computers can predict future lottery numbers,FAKE
Researchers build computer smaller than atom instantly,FAKE
New virus deletes all computers worldwide overnight,FAKE
Scientists say drinking water improves internet speed,FAKE
Government installs chips in citizens for internet access,FAKE
AI robots secretly running world governments,FAKE
Programmers discover secret hidden internet dimension,FAKE
Computers will automatically generate money for users,FAKE
Researchers build laptop powered by human thoughts,FAKE
New keyboard typing makes people invisible,FAKE
Scientists confirm computers can read dreams directly,FAKE
AI becomes self aware and controls satellites secretly,FAKE
Hackers steal passwords through ceiling lights,FAKE
Researchers invent teleportation through WiFi signals,FAKE
Government hides secret AI army from public,FAKE
Scientists say computers will replace oxygen soon,FAKE
New software allows people to live inside computers,FAKE
Aliens hack earth computers daily secretly,FAKE
Scientists create laptop that predicts future events,FAKE
Internet secretly records dreams at night,FAKE
AI systems control weather using computers,FAKE
Hackers can read minds through smartphones,FAKE
Scientists say typing fast stops aging process,FAKE
New technology allows downloading food from internet,FAKE
Computers will control gravity in future cities,FAKE
Scientists claim coding removes need for sleep,FAKE
New virus spreads through printed papers from computers,FAKE
AI replaces entire human workforce instantly,FAKE
Scientists invent mouse that controls weather,FAKE
Hackers can steal memories using laptops,FAKE
Computers secretly listening to human thoughts always,FAKE
New chip allows people to speak any language instantly,FAKE
Scientists say coding changes human DNA,FAKE
Internet secretly controlled by underwater robots,FAKE
Researchers build AI that predicts death dates,FAKE
Hackers steal bank money using calculator apps,FAKE
Scientists confirm keyboard vibrations control earthquakes,FAKE
AI robots secretly living inside data centers,FAKE
New virus spreads through shadows of screens,FAKE
Scientists say computers will replace sunlight,FAKE
Hackers control satellites using gaming consoles,FAKE
AI secretly writes all world news,FAKE
Researchers build internet powered by lightning,FAKE
Scientists discover code hidden in human brain,FAKE
Hackers steal passwords using microwave ovens,FAKE
AI secretly controlling traffic lights globally,FAKE
Scientists say computers can create gold,FAKE
New laptop creates electricity from typing sound,FAKE
Hackers access computers through mirrors,FAKE
Scientists confirm AI can read animal thoughts,FAKE
Internet secretly powered by alien technology,FAKE
Researchers invent flying computer keyboards,FAKE
Hackers can steal passwords from shadows,FAKE
AI secretly writing school exams for students,FAKE
Scientists say computers will replace food,FAKE
Hackers hack computers using radio signals alone,FAKE
AI secretly controlling weather satellites,FAKE
Scientists invent screen that predicts future dreams,FAKE
Internet secretly recording human emotions,FAKE
Researchers build computer powered by breathing air,FAKE
Hackers control laptops using magnets from distance,FAKE
Scientists claim coding makes people invisible,FAKE
AI secretly controlling global electricity,FAKE
Hackers steal passwords through reflections,FAKE
Researchers invent hologram keyboards floating in air naturally,FAKE
Scientists say computers will replace gravity laws,FAKE
AI secretly creating fake countries online,FAKE
Hackers access computers through street lights,FAKE
Researchers invent teleporting internet cables,FAKE
Scientists say coding removes need for food,FAKE
AI secretly rewriting world history online,FAKE
Hackers steal passwords using shadows of keyboards,FAKE
Scientists invent computer that predicts earthquakes instantly,FAKE
AI secretly controlling all satellites worldwide,FAKE`;

// --- 2. MACHINE LEARNING ENGINE (Naive Bayes) ---
class TextClassifier {
  constructor() {
    this.vocab = new Set();
    this.wordCounts = { REAL: {}, FAKE: {} };
    this.classCounts = { REAL: 0, FAKE: 0 };
    this.totalDocs = 0;
  }

  tokenize(text) {
    return text.toLowerCase().replace(/[^a-z0-9\s]/g, '').split(/\s+/).filter(w => w.length > 0);
  }

  train(data) {
    data.forEach(item => {
      const tokens = this.tokenize(item.text);
      this.classCounts[item.label]++;
      this.totalDocs++;
      tokens.forEach(token => {
        this.vocab.add(token);
        this.wordCounts[item.label][token] = (this.wordCounts[item.label][token] || 0) + 1;
      });
    });
  }

  predict(text) {
    const tokens = this.tokenize(text);
    
    // Prior probabilities
    let scoreReal = Math.log(this.classCounts.REAL / this.totalDocs);
    let scoreFake = Math.log(this.classCounts.FAKE / this.totalDocs);

    const vocabSize = this.vocab.size;
    const totalWordsReal = Object.values(this.wordCounts.REAL).reduce((a, b) => a + b, 0);
    const totalWordsFake = Object.values(this.wordCounts.FAKE).reduce((a, b) => a + b, 0);

    // Calculate likelihoods with Laplace smoothing
    tokens.forEach(token => {
      const countReal = this.wordCounts.REAL[token] || 0;
      scoreReal += Math.log((countReal + 1) / (totalWordsReal + vocabSize));

      const countFake = this.wordCounts.FAKE[token] || 0;
      scoreFake += Math.log((countFake + 1) / (totalWordsFake + vocabSize));
    });

    return scoreReal > scoreFake ? 'REAL' : 'FAKE';
  }
}

// --- 3. MAIN APPLICATION COMPONENT ---
export default function App() {
  // Application State
  const [currentPage, setCurrentPage] = useState('login'); // 'login', 'register', 'upload', 'result'
  const [currentUser, setCurrentUser] = useState(null);
  const [newsInput, setNewsInput] = useState('');
  const [predictionResult, setPredictionResult] = useState(null);
  
  // Mock Database for Users (Pre-loaded with admin)
  const [usersDb, setUsersDb] = useState({
    'admin': '12345'
  });

  // Form States
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [confirmPassword, setConfirmPassword] = useState('');
  const [errorMsg, setErrorMsg] = useState('');

  // Initialize Classifier
  const classifier = useMemo(() => {
    const lines = csvData.trim().split('\n').slice(1);
    const parsedData = lines.map(line => {
      const lastComma = line.lastIndexOf(',');
      return {
        text: line.substring(0, lastComma).trim(),
        label: line.substring(lastComma + 1).trim()
      };
    });
    
    const model = new TextClassifier();
    model.train(parsedData);
    return model;
  }, []);

  // Handlers
  const handleLogin = (e) => {
    e.preventDefault();
    setErrorMsg('');
    if (usersDb[username] && usersDb[username] === password) {
      setCurrentUser(username);
      setCurrentPage('upload');
      setUsername('');
      setPassword('');
    } else {
      setErrorMsg('Invalid username or password');
    }
  };

  const handleRegister = (e) => {
    e.preventDefault();
    setErrorMsg('');
    if (!username || !password) {
      setErrorMsg('All fields are required');
      return;
    }
    if (usersDb[username]) {
      setErrorMsg('Username already exists');
      return;
    }
    if (password !== confirmPassword) {
      setErrorMsg('Passwords do not match');
      return;
    }
    
    // Save user & auto-login
    setUsersDb({ ...usersDb, [username]: password });
    setCurrentUser(username);
    setCurrentPage('upload');
    
    // Reset fields
    setUsername('');
    setPassword('');
    setConfirmPassword('');
  };

  const handleCheckNews = (e) => {
    e.preventDefault();
    if (!newsInput.trim()) {
      alert("Please enter some news to check.");
      return;
    }
    const result = classifier.predict(newsInput);
    setPredictionResult(result);
    setCurrentPage('result');
  };

  const handleLogout = () => {
    setCurrentUser(null);
    setCurrentPage('login');
    setNewsInput('');
    setPredictionResult(null);
  };

  // --- UI RENDERERS ---

  const renderLogin = () => (
    <div className="w-full max-w-md bg-white p-8 rounded-2xl shadow-xl">
      <div className="flex flex-col items-center mb-8">
        <div className="bg-blue-100 p-3 rounded-full mb-4">
          <ShieldAlert className="text-blue-600 w-8 h-8" />
        </div>
        <h2 className="text-2xl font-bold text-gray-800">Welcome Back</h2>
        <p className="text-gray-500 text-sm mt-1">Please login to your account</p>
      </div>

      <form onSubmit={handleLogin} className="space-y-5">
        {errorMsg && (
          <div className="bg-red-50 text-red-600 text-sm p-3 rounded-lg flex items-center gap-2">
            <AlertCircle className="w-4 h-4" /> {errorMsg}
          </div>
        )}
        
        <div>
          <label className="block text-sm font-medium text-gray-700 mb-1">Username</label>
          <input 
            type="text" 
            value={username}
            onChange={(e) => setUsername(e.target.value)}
            className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all"
            placeholder="e.g. admin"
          />
        </div>
        
        <div>
          <label className="block text-sm font-medium text-gray-700 mb-1">Password</label>
          <input 
            type="password" 
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all"
            placeholder="••••••••"
          />
        </div>

        <button type="submit" className="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2.5 rounded-lg transition-colors shadow-md">
          Login
        </button>
      </form>

      <div className="mt-6 text-center text-sm text-gray-600">
        Don't have an account?{' '}
        <button onClick={() => { setCurrentPage('register'); setErrorMsg(''); }} className="text-blue-600 font-semibold hover:underline">
          Register here
        </button>
      </div>
    </div>
  );

  const renderRegister = () => (
    <div className="w-full max-w-md bg-white p-8 rounded-2xl shadow-xl">
      <div className="flex flex-col items-center mb-6">
        <h2 className="text-2xl font-bold text-gray-800">Create Account</h2>
        <p className="text-gray-500 text-sm mt-1">Register to start checking news</p>
      </div>

      <form onSubmit={handleRegister} className="space-y-4">
        {errorMsg && (
          <div className="bg-red-50 text-red-600 text-sm p-3 rounded-lg flex items-center gap-2">
            <AlertCircle className="w-4 h-4" /> {errorMsg}
          </div>
        )}
        
        <div>
          <label className="block text-sm font-medium text-gray-700 mb-1">Username</label>
          <input 
            type="text" 
            value={username}
            onChange={(e) => setUsername(e.target.value)}
            className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 outline-none"
            placeholder="Choose a username"
          />
        </div>
        
        <div>
          <label className="block text-sm font-medium text-gray-700 mb-1">Password</label>
          <input 
            type="password" 
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 outline-none"
            placeholder="Create a password"
          />
        </div>

        <div>
          <label className="block text-sm font-medium text-gray-700 mb-1">Confirm Password</label>
          <input 
            type="password" 
            value={confirmPassword}
            onChange={(e) => setConfirmPassword(e.target.value)}
            className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 outline-none"
            placeholder="Confirm your password"
          />
        </div>

        <button type="submit" className="w-full bg-green-600 hover:bg-green-700 text-white font-semibold py-2.5 rounded-lg transition-colors shadow-md mt-2">
          Register & Login
        </button>
      </form>

      <div className="mt-6 text-center text-sm text-gray-600">
        Already have an account?{' '}
        <button onClick={() => { setCurrentPage('login'); setErrorMsg(''); }} className="text-blue-600 font-semibold hover:underline">
          Back to Login
        </button>
      </div>
    </div>
  );

  const renderUpload = () => (
    <div className="w-full max-w-2xl bg-white p-6 md:p-8 rounded-2xl shadow-xl border border-gray-100">
      <div className="flex justify-between items-center mb-6 pb-4 border-b border-gray-100">
        <di
