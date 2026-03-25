import React, { useState, useEffect } from 'react';
import { 
  Heart, 
  Clock, 
  Calendar, 
  CheckCircle2, 
  ArrowRight, 
  MessageCircle, 
  Home, 
  Settings,
  ChevronRight,
  User,
  Quote
} from 'lucide-react';

const App = () => {
  const [step, setStep] = useState('welcome');
  const [userData, setUserData] = useState({
    name: '',
    familyMembers: '',
    biggestChallenge: '',
    lastQualityTime: ''
  });
  const [currentTask, setCurrentTask] = useState(null);
  const [completedTasks, setCompletedTasks] = useState([]);
  const [assessmentScore, setAssessmentScore] = useState(0);

  // Predefined task pool based on logic
  const taskPool = [
    {
      id: 1,
      title: "The 15-Minute Unplug",
      description: "Put your phone in another room as soon as you get home. Give your family your undivided attention for just 15 minutes.",
      impact: "Shows that they are more important than your emails.",
      category: "Presence"
    },
    {
      id: 2,
      title: "The Appreciation Note",
      description: "Write a short, handwritten note (not a text) to your spouse mentioning one thing they did this week that you appreciate.",
      impact: "Validates their efforts in maintaining the household.",
      category: "Communication"
    },
    {
      id: 3,
      title: "The 'No-Work' Dinner",
      description: "Have one meal where business talk is strictly forbidden. Ask about their dreams or something they learned today.",
      impact: "Rebuilds emotional intimacy.",
      category: "Connection"
    },
    {
      id: 4,
      title: "The Calendar Commit",
      description: "Block out 2 hours this Saturday in your work calendar as 'Mandatory Meeting' but spend it taking a walk with your partner.",
      impact: "Prioritizes family at a structural level.",
      category: "Time Management"
    }
  ];

  const handleStart = () => setStep('assessment');

  const handleAssessmentSubmit = (e) => {
    e.preventDefault();
    setStep('analyzing');
    setTimeout(() => {
      setCurrentTask(taskPool[0]);
      setStep('dashboard');
    }, 2000);
  };

  const completeTask = () => {
    setCompletedTasks([...completedTasks, currentTask.id]);
    const nextIndex = taskPool.findIndex(t => t.id === currentTask.id) + 1;
    if (nextIndex < taskPool.length) {
      setCurrentTask(taskPool[nextIndex]);
      setStep('task-transition');
    } else {
      setStep('milestone');
    }
  };

  // UI Components
  const WelcomeView = () => (
    <div className="flex flex-col items-center justify-center min-h-[80vh] text-center p-6">
      <div className="bg-indigo-100 p-4 rounded-full mb-6">
        <Heart className="w-12 h-12 text-indigo-600" />
      </div>
      <h1 className="text-3xl font-serif font-bold text-slate-800 mb-4">The Bridge</h1>
      <p className="text-slate-600 max-w-md mb-8 leading-relaxed">
        Success in business is hollow without a home to share it with. 
        We help busy leaders rebuild the most important partnership of their lives.
      </p>
      <button 
        onClick={handleStart}
        className="bg-indigo-600 text-white px-8 py-4 rounded-xl font-semibold shadow-lg hover:bg-indigo-700 transition-all flex items-center gap-2"
      >
        Begin Reconciliation <ArrowRight className="w-5 h-5" />
      </button>
    </div>
  );

  const AssessmentView = () => (
    <div className="max-w-md mx-auto p-6">
      <h2 className="text-2xl font-bold text-slate-800 mb-6">Let's Understand Your Situation</h2>
      <form onSubmit={handleAssessmentSubmit} className="space-y-6">
        <div>
          <label className="block text-sm font-medium text-slate-700 mb-2">Your Name</label>
          <input 
            required
            className="w-full p-3 border border-slate-200 rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none"
            placeholder="e.g. Robert"
            onChange={e => setUserData({...userData, name: e.target.value})}
          />
        </div>
        <div>
          <label className="block text-sm font-medium text-slate-700 mb-2">Who are you trying to reconnect with?</label>
          <input 
            required
            className="w-full p-3 border border-slate-200 rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none"
            placeholder="e.g. My wife and two kids"
            onChange={e => setUserData({...userData, familyMembers: e.target.value})}
          />
        </div>
        <div>
          <label className="block text-sm font-medium text-slate-700 mb-2">What is the biggest friction point right now?</label>
          <textarea 
            required
            className="w-full p-3 border border-slate-200 rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none"
            rows="3"
            placeholder="e.g. I'm always on my phone or late for dinner"
            onChange={e => setUserData({...userData, biggestChallenge: e.target.value})}
          ></textarea>
        </div>
        <button 
          type="submit"
          className="w-full bg-slate-800 text-white py-4 rounded-xl font-semibold hover:bg-slate-900 transition-all"
        >
          Create My Reconnection Plan
        </button>
      </form>
    </div>
  );

  const AnalyzingView = () => (
    <div className="flex flex-col items-center justify-center min-h-[60vh] p-6 text-center">
      <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-indigo-600 mb-4"></div>
      <h2 className="text-xl font-semibold text-slate-800">Analyzing your priorities...</h2>
      <p className="text-slate-500">Creating a sustainable path back to your family.</p>
    </div>
  );

  const DashboardView = () => (
    <div className="p-6 max-w-2xl mx-auto pb-24">
      <div className="flex justify-between items-center mb-8">
        <div>
          <h2 className="text-sm font-bold text-indigo-600 uppercase tracking-wider">Current Focus</h2>
          <h1 className="text-2xl font-bold text-slate-900">Welcome, {userData.name}</h1>
        </div>
        <div className="bg-slate-100 p-2 rounded-lg">
          <User className="w-6 h-6 text-slate-600" />
        </div>
      </div>

      {currentTask && (
        <div className="bg-white border border-slate-100 shadow-xl rounded-2xl overflow-hidden mb-8">
          <div className="bg-indigo-600 p-4 flex items-center justify-between">
            <span className="text-white font-medium flex items-center gap-2">
              <Clock className="w-4 h-4" /> Active Task
            </span>
            <span className="bg-indigo-500 text-indigo-100 text-xs px-2 py-1 rounded">
              {currentTask.category}
            </span>
          </div>
          <div className="p-6">
            <h3 className="text-xl font-bold text-slate-800 mb-3">{currentTask.title}</h3>
            <p className="text-slate-600 mb-6 leading-relaxed italic border-l-4 border-indigo-100 pl-4">
              "{currentTask.description}"
            </p>
            <div className="bg-slate-50 p-4 rounded-xl mb-6">
              <p className="text-sm font-semibold text-slate-500 mb-1">WHY THIS WORKS</p>
              <p className="text-slate-700">{currentTask.impact}</p>
            </div>
            <button 
              onClick={completeTask}
              className="w-full bg-indigo-600 text-white py-4 rounded-xl font-bold flex items-center justify-center gap-2 hover:bg-indigo-700"
            >
              <CheckCircle2 className="w-5 h-5" /> I have completed this task
            </button>
          </div>
        </div>
      )}

      <div className="grid grid-cols-2 gap-4 mb-8">
        <div className="bg-white p-4 rounded-xl border border-slate-100 shadow-sm">
          <p className="text-xs font-bold text-slate-400 uppercase mb-1">Completed</p>
          <p className="text-2xl font-bold text-slate-800">{completedTasks.length}</p>
        </div>
        <div className="bg-white p-4 rounded-xl border border-slate-100 shadow-sm">
          <p className="text-xs font-bold text-slate-400 uppercase mb-1">Status</p>
          <p className="text-sm font-bold text-orange-500">Rebuilding</p>
        </div>
      </div>

      <div className="bg-indigo-50 p-4 rounded-xl flex items-start gap-3">
        <Quote className="w-5 h-5 text-indigo-400 mt-1" />
        <p className="text-indigo-800 text-sm italic">
          "No amount of money can buy back a missed childhood or a lost love. Start small today."
        </p>
      </div>
    </div>
  );

  const TransitionView = () => (
    <div className="flex flex-col items-center justify-center min-h-[80vh] p-6 text-center">
      <div className="w-20 h-20 bg-green-100 rounded-full flex items-center justify-center mb-6">
        <CheckCircle2 className="w-10 h-10 text-green-600" />
      </div>
      <h2 className="text-2xl font-bold text-slate-800 mb-2">Excellent Progress</h2>
      <p className="text-slate-600 mb-8">You are showing your family that they are a priority. Consistency is key.</p>
      <button 
        onClick={() => setStep('dashboard')}
        className="bg-indigo-600 text-white px-8 py-4 rounded-xl font-bold"
      >
        View Next Step
      </button>
    </div>
  );

  return (
    <div className="min-h-screen bg-slate-50 font-sans text-slate-900">
      {/* Navigation */}
      <nav className="bg-white border-b border-slate-100 p-4 flex justify-between items-center sticky top-0 z-10">
        <div className="flex items-center gap-2">
          <Heart className="text-indigo-600 w-6 h-6 fill-current" />
          <span className="font-serif font-bold text-xl text-slate-800 tracking-tight">The Bridge</span>
        </div>
      </nav>

      {/* Main Content Area */}
      <main className="max-w-4xl mx-auto">
        {step === 'welcome' && <WelcomeView />}
        {step === 'assessment' && <AssessmentView />}
        {step === 'analyzing' && <AnalyzingView />}
        {step === 'dashboard' && <DashboardView />}
        {step === 'task-transition' && <TransitionView />}
        {step === 'milestone' && (
          <div className="p-6 text-center mt-20">
            <h1 className="text-3xl font-bold mb-4">You've finished the first phase!</h1>
            <p className="mb-8">Check back tomorrow for your next customized action plan.</p>
            <button onClick={() => setStep('dashboard')} className="text-indigo-600 font-bold underline">Go Back to Dashboard</button>
          </div>
        )}
      </main>

      {/* Mobile Bottom Bar (Simulated) */}
      {step === 'dashboard' && (
        <div className="fixed bottom-0 left-0 right-0 bg-white border-t border-slate-100 p-4 flex justify-around items-center">
          <button className="flex flex-col items-center text-indigo-600">
            <Home className="w-6 h-6" />
            <span className="text-[10px] font-bold mt-1">Focus</span>
          </button>
          <button className="flex flex-col items-center text-slate-400">
            <Calendar className="w-6 h-6" />
            <span className="text-[10px] font-bold mt-1">History</span>
          </button>
          <button className="flex flex-col items-center text-slate-400">
            <MessageCircle className="w-6 h-6" />
            <span className="text-[10px] font-bold mt-1">Guidance</span>
          </button>
          <button className="flex flex-col items-center text-slate-400">
            <Settings className="w-6 h-6" />
            <span className="text-[10px] font-bold mt-1">Profile</span>
          </button>
        </div>
      )}
    </div>
  );
};

export default App;
