<script>
  let recognition;
  let transcription = '';
  let listening = false;

  if ('webkitSpeechRecognition' in window) {
    recognition = new webkitSpeechRecognition();
    recognition.continuous = true;
    recognition.interimResults = true;

    recognition.onresult = function(event) {
      transcription = '';
      for (let i = event.resultIndex; i < event.results.length; ++i) {
        transcription += event.results[i][0].transcript;
      }
    };

    recognition.onerror = function(event) {
      console.error('Speech recognition error', event);
    };

    recognition.onend = function() {
      listening = false;
    };
  } else {
    console.error("Your browser doesn't support speech recognition.");
  }

  function toggleListening() {
    if (listening) {
      recognition.stop();
    } else {
      recognition.start();
    }
    listening = !listening;
  }
</script>

<button on:click={toggleListening}>
  {listening ? 'Stop Listening' : 'Start Listening'}
</button>
<p>Transcription: {transcription}</p>
