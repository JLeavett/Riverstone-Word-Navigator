import React, { useState, useEffect, useCallback, useRef } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { Play, RotateCcw, Volume2, VolumeX } from 'lucide-react';
import { Button } from '@/components/ui/button';

const VOWELS = ['A', 'E', 'I', 'O', 'U', 'a', 'e', 'i', 'o', 'u'];
const CONSONANTS = ['B', 'C', 'D', 'F', 'G', 'H', 'J', 'K', 'L', 'M', 'N', 'P', 'Q', 'R', 'S', 'T', 'V', 'W', 'X', 'Y', 'Z',
                    'b', 'c', 'd', 'f', 'g', 'h', 'j', 'k', 'l', 'm', 'n', 'p', 'q', 'r', 's', 't', 'v', 'w', 'x', 'y', 'z'];

const FISH_COLORS = [
  { body: '#FF6B6B', fin: '#FF8E8E', belly: '#FFB5B5' },
  { body: '#4ECDC4', fin: '#6EE7DF', belly: '#A8F0EB' },
  { body: '#FFE66D', fin: '#FFF09E', belly: '#FFF7C7' },
  { body: '#95E1D3', fin: '#B5F0E5', belly: '#D4F8F2' },
  { body: '#F38181', fin: '#F5A3A3', belly: '#F8C5C5' },
  { body: '#AA96DA', fin: '#C4B5E8', belly: '#DED4F4' },
  { body: '#FCBAD3', fin: '#FDCDE1', belly: '#FEE0EE' },
];

const FlyingFish = ({ fish, onFishClick, gameActive }) => {
  const [rotation, setRotation] = useState(0);
  const isVowel = VOWELS.includes(fish.letter);
  const color = FISH_COLORS[fish.colorIndex];
  
  // Calculate the arc path
  const startX = fish.startX;
  const endX = fish.endX;
  const peakHeight = fish.peakHeight;
  const duration = fish.duration;
  
  const direction = endX > startX ? 1 : -1;
  
  // Provide rotation setter to parent
  React.useEffect(() => {
    fish.rotation = rotation;
    fish.setRotation = setRotation;
  }, [rotation, fish]);
  
  return (
    <motion.div
      className="absolute cursor-pointer select-none"
      style={{ 
        left: startX,
        bottom: 180,
        zIndex: 20,
      }}
      initial={{ 
        x: 0, 
        y: 0,
        rotate: direction === 1 ? -30 : 30,
        scale: 0.8,
        opacity: 0.3
      }}
      animate={{ 
        x: [0, (endX - startX) * 0.5, endX - startX],
        y: [0, -peakHeight, 0],
        rotate: [
          direction === 1 ? -35 : 35,
          direction === 1 ? -5 : 5,
          direction === 1 ? 35 : -35
        ],
        scale: [0.8, 1, 0.8],
        opacity: [0.3, 1, 0.3]
      }}
      onUpdate={(latest) => {
        // Store rotation for letter counter-rotation
        if (fish.setRotation) {
          fish.setRotation(latest.rotate || 0);
        }
      }}
      transition={{ 
        duration: duration,
        ease: "easeInOut",
        times: [0, 0.5, 1]
      }}
      onClick={(e) => {
        e.stopPropagation();
        if (gameActive && fish.clicksRemaining > 0) {
          onFishClick(fish.id, isVowel);
        }
      }}
    >
      <svg 
        width="120" 
        height="80" 
        viewBox="0 0 120 80" 
        className="drop-shadow-lg hover:drop-shadow-2xl transition-all"
        style={{ transform: direction === -1 ? 'scaleX(-1)' : 'none' }}
      >
        {/* Main body */}
        <ellipse cx="55" cy="40" rx="40" ry="22" fill={color.body} />
        
        {/* Belly */}
        <ellipse cx="55" cy="48" rx="32" ry="12" fill={color.belly} opacity="0.7" />
        
        {/* Tail */}
        <path d="M15 40 L-5 20 L5 40 L-5 60 Z" fill={color.fin} />
        
        {/* Top fin (wing) */}
        <path d="M45 18 Q60 -15 90 10 Q70 20 55 22" fill={color.fin} />
        
        {/* Bottom fin (wing) */}
        <path d="M50 58 Q65 85 85 65 Q70 60 55 58" fill={color.fin} opacity="0.9" />
        
        {/* Side fin */}
        <ellipse cx="65" cy="45" rx="8" ry="5" fill={color.fin} />
        
        {/* Eye */}
        <circle cx="80" cy="35" r="8" fill="white" />
        <circle cx="82" cy="34" r="5" fill="#2D3436" />
        <circle cx="84" cy="32" r="2" fill="white" />
        
        {/* Mouth */}
        <path d="M95 42 Q98 44 95 46" stroke="#2D3436" strokeWidth="2" fill="none" />
        
        {/* Letter circle */}
        <g transform={direction === -1 ? 'scale(-1, 1) translate(-110, 0)' : ''}>
          <circle cx="55" cy="40" r="18" fill="white" opacity="0.95" />
          <text 
            x="55" 
            y="47" 
            textAnchor="middle" 
            fontSize="24" 
            fontFamily="Lexend, sans-serif"
            fontWeight="700"
            fill="#1a1a2e"
            transform={`rotate(${-fish.rotation || 0} 55 40)`}
          >
            {fish.letter}
          </text>
        </g>
      </svg>
    </motion.div>
  );
};

const Splash = ({ x, type = 'entry' }) => (
  <motion.div
    className="absolute bottom-[265px] pointer-events-none"
    style={{ left: x }}
    initial={{ opacity: 1, scale: 0.5 }}
    animate={{ opacity: 0, scale: 1.5 }}
    exit={{ opacity: 0 }}
    transition={{ duration: 0.4 }}
  >
    <svg width="60" height="40" viewBox="0 0 60 40">
      <ellipse cx="30" cy="30" rx="25" ry="8" fill="#FFFFFF" opacity="0.7" />
      <circle cx="15" cy="20" r="4" fill="#FFFFFF" opacity="0.8" />
      <circle cx="30" cy="10" r="5" fill="#FFFFFF" opacity="0.8" />
      <circle cx="45" cy="18" r="4" fill="#FFFFFF" opacity="0.8" />
      {/* Ripples */}
      <ellipse cx="30" cy="32" rx="20" ry="4" fill="none" stroke="#FFFFFF" strokeWidth="1" opacity="0.5" />
    </svg>
  </motion.div>
);

const FeedbackPopup = ({ feedback }) => (
  <motion.div
    className="absolute pointer-events-none font-bold text-2xl"
    style={{ 
      left: feedback.x, 
      top: feedback.y,
      fontFamily: 'Lexend, sans-serif'
    }}
    initial={{ opacity: 1, y: 0, scale: 1 }}
    animate={{ opacity: 0, y: -50, scale: 1.2 }}
    exit={{ opacity: 0 }}
    transition={{ duration: 0.6 }}
  >
    <span className={feedback.correct ? 'text-emerald-400' : 'text-rose-400'}>
      {feedback.correct ? '+5' : '-4'}
    </span>
  </motion.div>
);

export default function VowelFishGame() {
  const [gameState, setGameState] = useState('start'); // start, playing, ended
  const [score, setScore] = useState(0);
  const [timeLeft, setTimeLeft] = useState(60);
  const [fish, setFish] = useState([]);
  const [splashes, setSplashes] = useState([]);
  const [feedbacks, setFeedbacks] = useState([]);
  const [stats, setStats] = useState({ correct: 0, incorrect: 0 });
  const fishIdRef = useRef(0);
  const containerRef = useRef(null);

  const getRandomLetter = () => {
    const useVowel = Math.random() < 0.4; // 40% chance of vowel
    const letters = useVowel ? VOWELS : CONSONANTS;
    return letters[Math.floor(Math.random() * letters.length)];
  };

  const spawnFish = useCallback(() => {
    if (!containerRef.current) return null;
    
    const containerWidth = containerRef.current.offsetWidth;
    const margin = 100;
    const direction = Math.random() > 0.5 ? 1 : -1;
    
    const startX = direction === 1 
      ? margin + Math.random() * (containerWidth / 3 - margin)
      : containerWidth - margin - Math.random() * (containerWidth / 3 - margin);
    
    const jumpDistance = 300 + Math.random() * 250;
    const endX = startX + (direction * jumpDistance);
    
    const newFish = {
      id: fishIdRef.current++,
      letter: getRandomLetter(),
      startX: startX,
      endX: Math.max(margin, Math.min(containerWidth - margin, endX)),
      peakHeight: 120 + Math.random() * 80,
      duration: 3.5 + Math.random() * 1.5,
      colorIndex: Math.floor(Math.random() * FISH_COLORS.length),
      clicksRemaining: 2,
      spawnTime: Date.now()
    };
    
    return newFish;
  }, []);

  const handleFishClick = useCallback((fishId, isVowel) => {
    setFish(prev => {
      const fishIndex = prev.findIndex(f => f.id === fishId);
      if (fishIndex === -1) return prev;
      
      const clickedFish = prev[fishIndex];
      if (clickedFish.clicksRemaining <= 0) return prev;
      
      return prev.map(f => 
        f.id === fishId 
          ? { ...f, clicksRemaining: f.clicksRemaining - 1 }
          : f
      );
    });

    const points = isVowel ? 5 : -4;
    setScore(prev => Math.max(0, prev + points));
    setStats(prev => ({
      correct: prev.correct + (isVowel ? 1 : 0),
      incorrect: prev.incorrect + (isVowel ? 0 : 1)
    }));

    // Show feedback
    const feedbackId = Date.now();
    setFeedbacks(prev => [...prev, {
      id: feedbackId,
      x: Math.random() * 200 + 100,
      y: Math.random() * 100 + 100,
      correct: isVowel
    }]);
    
    setTimeout(() => {
      setFeedbacks(prev => prev.filter(f => f.id !== feedbackId));
    }, 700);
  }, []);

  // Game timer
  useEffect(() => {
    if (gameState !== 'playing') return;
    
    const timer = setInterval(() => {
      setTimeLeft(prev => {
        if (prev <= 1) {
          setGameState('ended');
          return 0;
        }
        return prev - 1;
      });
    }, 1000);

    return () => clearInterval(timer);
  }, [gameState]);

  // Fish spawning and cleanup
  useEffect(() => {
    if (gameState !== 'playing') return;

    const spawnInterval = setInterval(() => {
      setFish(prev => {
        // Remove fish that have completed their animation
        const now = Date.now();
        const activeFish = prev.filter(f => now - f.spawnTime < f.duration * 1000);
        
        // Spawn new fish if under limit
        if (activeFish.length < 3) {
          const newFish = spawnFish();
          if (newFish) {
            // Add entry splash effect
            const entrySplashId = Date.now();
            setSplashes(prev => [...prev, { id: entrySplashId, x: newFish.startX, type: 'entry' }]);
            setTimeout(() => {
              setSplashes(prev => prev.filter(s => s.id !== entrySplashId));
            }, 500);
            
            // Add exit splash effect at the end
            setTimeout(() => {
              const exitSplashId = Date.now() + 1;
              setSplashes(prev => [...prev, { id: exitSplashId, x: newFish.endX, type: 'exit' }]);
              setTimeout(() => {
                setSplashes(prev => prev.filter(s => s.id !== exitSplashId));
              }, 500);
            }, newFish.duration * 1000);
            
            return [...activeFish, newFish];
          }
        }
        return activeFish;
      });
    }, 800);

    return () => clearInterval(spawnInterval);
  }, [gameState, spawnFish]);

  const startGame = () => {
    setGameState('playing');
    setScore(0);
    setTimeLeft(60);
    setFish([]);
    setStats({ correct: 0, incorrect: 0 });
    fishIdRef.current = 0;
  };

  return (
    <div className="min-h-screen bg-gradient-to-b from-slate-900 via-slate-800 to-slate-900 overflow-hidden">
      {/* Import Lexend font */}
      <style>
        {`@import url('https://fonts.googleapis.com/css2?family=Lexend:wght@400;500;600;700;800&display=swap');`}
      </style>
      
      <div 
        ref={containerRef}
        className="relative w-full h-screen max-h-[800px] overflow-hidden"
      >
        {/* Sky gradient */}
        <div className="absolute inset-0 bg-gradient-to-b from-sky-300 via-blue-300 to-indigo-400" />
        
        {/* Sun */}
        <div className="absolute top-8 right-12 w-24 h-24 rounded-full bg-gradient-to-br from-yellow-200 to-orange-300 blur-sm opacity-90" />
        <div className="absolute top-10 right-14 w-20 h-20 rounded-full bg-gradient-to-br from-yellow-100 to-yellow-200" />
        
        {/* Clouds */}
        <div className="absolute top-16 left-[10%] w-32 h-10 bg-white/40 rounded-full blur-md" />
        <div className="absolute top-12 left-[15%] w-24 h-8 bg-white/30 rounded-full blur-md" />
        <div className="absolute top-20 left-[60%] w-40 h-12 bg-white/35 rounded-full blur-md" />
        
        {/* Distant trees/hills */}
        <div className="absolute bottom-[300px] left-0 right-0 h-20 bg-gradient-to-t from-emerald-800/60 to-transparent" />
        
        {/* River bank */}
        <div className="absolute bottom-[270px] left-0 w-full h-8 bg-gradient-to-r from-amber-700 via-amber-600/50 to-amber-700" 
             style={{ clipPath: 'polygon(0 100%, 0 0, 15% 50%, 30% 20%, 50% 60%, 70% 30%, 85% 50%, 100% 0, 100% 100%)' }} />
        
        {/* Water - deeper and blue */}
        <div className="absolute bottom-0 left-0 right-0 h-[280px] bg-gradient-to-b from-blue-400 via-blue-500 to-blue-700">
          {/* Water ripples */}
          <div className="absolute inset-0 overflow-hidden">
            {[...Array(8)].map((_, i) => (
              <motion.div
                key={i}
                className="absolute h-[2px] bg-white/20 rounded-full"
                style={{
                  width: 60 + Math.random() * 100,
                  left: `${Math.random() * 100}%`,
                  top: `${20 + Math.random() * 60}%`,
                }}
                animate={{
                  x: [-20, 20, -20],
                  opacity: [0.1, 0.3, 0.1],
                }}
                transition={{
                  duration: 3 + Math.random() * 2,
                  repeat: Infinity,
                  delay: Math.random() * 2,
                }}
              />
            ))}
          </div>
          
          {/* Water shine */}
          <div className="absolute top-4 left-1/4 w-48 h-3 bg-white/10 rounded-full blur-sm" />
          <div className="absolute top-8 right-1/3 w-32 h-2 bg-white/15 rounded-full blur-sm" />
        </div>
        
        {/* Flying fish */}
        <AnimatePresence>
          {fish.map(f => (
            <FlyingFish
              key={f.id}
              fish={f}
              onFishClick={handleFishClick}
              gameActive={gameState === 'playing'}
            />
          ))}
        </AnimatePresence>
        
        {/* Splashes */}
        <AnimatePresence>
          {splashes.map(s => (
            <Splash key={s.id} x={s.x} />
          ))}
        </AnimatePresence>
        
        {/* Score feedbacks */}
        <AnimatePresence>
          {feedbacks.map(f => (
            <FeedbackPopup key={f.id} feedback={f} />
          ))}
        </AnimatePresence>
        
        {/* UI Overlay */}
        <div className="absolute top-0 left-0 right-0 p-4 flex justify-between items-start z-30">
          {/* Score */}
          <div className="bg-slate-900/70 backdrop-blur-md rounded-2xl px-6 py-3 border border-white/10">
            <p className="text-white/60 text-xs font-medium uppercase tracking-wider" style={{ fontFamily: 'Lexend' }}>Score</p>
            <p className="text-3xl font-bold text-white" style={{ fontFamily: 'Lexend' }}>{score}</p>
          </div>
          
          {/* Timer */}
          <div className="bg-slate-900/70 backdrop-blur-md rounded-2xl px-6 py-3 border border-white/10">
            <p className="text-white/60 text-xs font-medium uppercase tracking-wider" style={{ fontFamily: 'Lexend' }}>Time</p>
            <p className={`text-3xl font-bold ${timeLeft <= 10 ? 'text-rose-400' : 'text-white'}`} style={{ fontFamily: 'Lexend' }}>
              {timeLeft}s
            </p>
          </div>
        </div>
        
        {/* Instructions during gameplay */}
        {gameState === 'playing' && (
          <div className="absolute top-20 left-1/2 -translate-x-1/2 bg-slate-900/60 backdrop-blur-sm rounded-full px-6 py-2 border border-white/10">
            <p className="text-white/80 text-sm font-medium" style={{ fontFamily: 'Lexend' }}>
              Click the fish with <span className="text-emerald-400 font-bold">VOWELS</span> (upper and lower case)
            </p>
          </div>
        )}
        
        {/* Start Screen */}
        {gameState === 'start' && (
          <motion.div 
            className="absolute inset-0 flex items-center justify-center z-40 bg-slate-900/40 backdrop-blur-sm"
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
          >
            <div className="bg-slate-900/90 backdrop-blur-xl rounded-3xl p-8 max-w-md mx-4 border border-white/10 shadow-2xl">
              <h1 className="text-4xl font-extrabold text-white mb-2 text-center" style={{ fontFamily: 'Lexend' }}>
                Vowel Fish
              </h1>
              <p className="text-white/60 text-center mb-6" style={{ fontFamily: 'Lexend' }}>
                Catch the flying fish!
              </p>
              
              <div className="bg-slate-800/50 rounded-2xl p-4 mb-6 space-y-3">
                <p className="text-white/90 text-sm" style={{ fontFamily: 'Lexend' }}>
                  🎯 Click fish with <span className="text-emerald-400 font-bold">vowels</span> (upper and lower case)
                </p>
                <p className="text-white/90 text-sm" style={{ fontFamily: 'Lexend' }}>
                  ✅ Correct: <span className="text-emerald-400 font-bold">+5 points</span>
                </p>
                <p className="text-white/90 text-sm" style={{ fontFamily: 'Lexend' }}>
                  ❌ Wrong: <span className="text-rose-400 font-bold">-4 points</span>
                </p>
                <p className="text-white/90 text-sm" style={{ fontFamily: 'Lexend' }}>
                  ⏱️ You have <span className="text-amber-400 font-bold">60 seconds</span>
                </p>
              </div>
              
              <Button 
                onClick={startGame}
                className="w-full bg-gradient-to-r from-emerald-500 to-teal-500 hover:from-emerald-600 hover:to-teal-600 text-white font-bold py-6 rounded-xl text-lg shadow-lg shadow-emerald-500/25 transition-all hover:scale-[1.02]"
                style={{ fontFamily: 'Lexend' }}
              >
                <Play className="w-5 h-5 mr-2" />
                Start Game
              </Button>
            </div>
          </motion.div>
        )}
        
        {/* End Screen */}
        {gameState === 'ended' && (
          <motion.div 
            className="absolute inset-0 flex items-center justify-center z-40 bg-slate-900/40 backdrop-blur-sm"
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
          >
            <div className="bg-slate-900/90 backdrop-blur-xl rounded-3xl p-8 max-w-md mx-4 border border-white/10 shadow-2xl">
              <h1 className="text-4xl font-extrabold text-white mb-2 text-center" style={{ fontFamily: 'Lexend' }}>
                Time's Up!
              </h1>
              
              <div className="text-center my-8">
                <p className="text-white/60 text-sm uppercase tracking-wider mb-2" style={{ fontFamily: 'Lexend' }}>Final Score</p>
                <p className="text-6xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-emerald-400 to-teal-400" style={{ fontFamily: 'Lexend' }}>
                  {score}
                </p>
              </div>
              
              <div className="grid grid-cols-2 gap-4 mb-8">
                <div className="bg-emerald-500/20 rounded-xl p-4 text-center border border-emerald-500/30">
                  <p className="text-3xl font-bold text-emerald-400" style={{ fontFamily: 'Lexend' }}>{stats.correct}</p>
                  <p className="text-emerald-300/80 text-xs uppercase tracking-wider" style={{ fontFamily: 'Lexend' }}>Correct</p>
                </div>
                <div className="bg-rose-500/20 rounded-xl p-4 text-center border border-rose-500/30">
                  <p className="text-3xl font-bold text-rose-400" style={{ fontFamily: 'Lexend' }}>{stats.incorrect}</p>
                  <p className="text-rose-300/80 text-xs uppercase tracking-wider" style={{ fontFamily: 'Lexend' }}>Mistakes</p>
                </div>
              </div>
              
              <div className="bg-slate-800/50 rounded-xl p-4 mb-6 text-center">
                <p className="text-white/80 text-sm" style={{ fontFamily: 'Lexend' }}>
                  {score >= 50 ? '🏆 Amazing! You\'re a vowel master!' :
                   score >= 30 ? '⭐ Great job! Keep practicing!' :
                   score >= 15 ? '👍 Good effort! Try again!' :
                   '💪 Keep learning! You\'ll get better!'}
                </p>
              </div>
              
              <Button 
                onClick={startGame}
                className="w-full bg-gradient-to-r from-purple-500 to-pink-500 hover:from-purple-600 hover:to-pink-600 text-white font-bold py-6 rounded-xl text-lg shadow-lg shadow-purple-500/25 transition-all hover:scale-[1.02]"
                style={{ fontFamily: 'Lexend' }}
              >
                <RotateCcw className="w-5 h-5 mr-2" />
                Play Again
              </Button>
            </div>
          </motion.div>
        )}
      </div>
    </div>
  );
}
