<script>
    import { onMount } from 'svelte';
  
    let languages = [
      { name: 'Je parle français', code: 'fr' },
      { name: 'I speak english', code: 'en' },
      { name: 'Yo hablo español', code: 'es' },
      { name: 'Ich spreche Deutsch', code: 'de' }
    ];
  
    let currentLanguage = {};
    let recognition = null;
    let scoreGame3 = 0;
    let maxPoints = languages.length * 100;
    let voice = null;
    let isListening = false;
  
    function startListening() {
      recognition = new window.webkitSpeechRecognition();
      recognition.lang = currentLanguage.code;
      recognition.start();
      recognition.onresult = event => {
        const spokenLanguage = event.results[0][0].transcript.toLowerCase();
        const result = checkAnswer(spokenLanguage);
        if (result.correct) {
          scoreGame3 += 100;
          alert('Correct! You earned 100 points');
        } else {
          alert('Incorrect! The correct answer is ' + result.expectedLanguage);
        }
        localStorage.setItem('scoreGame3', scoreGame3);
      }
      recognition.onend = () => {
        isListening = false;
      }
    }
  
    async function setupVoice() {
      const voices = await window.speechSynthesis.getVoices();
      voice = voices.find(v => v.lang === currentLanguage.code);
    }
  
    function speakLanguage() {
      const utterance = new SpeechSynthesisUtterance(currentLanguage.name);
      utterance.voice = voice;
      window.speechSynthesis.speak(utterance);
    }
  
    function checkAnswer(spokenLanguage) {
      const expectedLanguage = currentLanguage.name.toLowerCase();
      const correct = spokenLanguage.includes(expectedLanguage);
      return {
        correct: correct,
        expectedLanguage: expectedLanguage
      };
    }
  
    function getNextLanguage() {
      const unusedLanguages = languages.filter(language => language.code !== currentLanguage.code);
      currentLanguage = unusedLanguages[Math.floor(Math.random() * unusedLanguages.length)];
      return currentLanguage;
    }
  
    onMount(() => {
      const savedscoreGame3 = localStorage.getItem('scoreGame3');
      scoreGame3 = savedscoreGame3 ? parseInt(savedscoreGame3) : 0;
      getNextLanguage();
      setupVoice();
    });
  </script>
  
  <h1>Guess the Language</h1>
  
  {#if isListening}
    <button on:click={recognition.stop}>Stop Listening</button>
  {:else}
    <button on:click={speakLanguage}>Speak Language</button>
    <button on:click={startListening}>Start Listening</button>
  {/if}
  
  <p>Your score: {scoreGame3} / {maxPoints}</p>
  <p>Next language: {currentLanguage.name}</p>
  