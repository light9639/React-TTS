# ποΈ React λ§λ  Text To Speech μμ  νμΌμλλ€.
:octocat: https://light9639.github.io/React-TTS/

![light9639 github io_React-TTS_](https://user-images.githubusercontent.com/95972251/212852447-75ab10d7-7671-415d-8fad-2c04265ebfd5.png)

:sparkles: React λ§λ  Text To Speech μμ  νμΌμλλ€. :sparkles:
## :tada: React μμ±
- React μμ±
```bash
npm create-react-app my-app
# or
yarn create react-app my-app
```

- viteλ₯Ό μ΄μ©νμ¬ νλ‘μ νΈλ₯Ό μμ±νλ €λ©΄
```bash
npm create vite@latest
# or
yarn create vite
```
- ν°λ―Έλμμ μ€ν ν νλ‘μ νΈ μ΄λ¦ λ§λ  ν React μ ν, Typescirpt μ ννλ©΄ μμ± μλ£.

## βοΈ App.tsx, getSpeech.ts μμ  λ° μμ±
### :zap: App.tsx
- `useEffect` μμ `window.speechSynthesis.getVoices();`λ₯Ό ν΅ν΄ μμ± λ³νμ ν  μ μλ€.
```js
import { useEffect, useState } from 'react'
import reactLogo from './assets/react.svg'
import './App.css';
import { getSpeech } from "./utils/getSpeech";

function App() {
  const [value, setValue] = useState("μλνμΈμ");

  //μμ± λ³ν λͺ©μλ¦¬ preload
  useEffect(() => {
    window.speechSynthesis.getVoices();
  }, []);

  const handleInput = (e: { target: { value: any; }; }) => {
    const { value } = e.target;
    setValue(value);
  };

  const handleButton = () => {
    getSpeech(value);
  };

  return (
    <div className="App">
      <div>
        <a href="https://vitejs.dev" target="_blank">
          <img src="/vite.svg" className="logo" alt="Vite logo" />
        </a>
        <a href="https://reactjs.org" target="_blank">
          <img src={reactLogo} className="logo react" alt="React logo" />
        </a>
      </div>
      <h1>TTS(νκ΅­μ΄)</h1>
      <p>νμ€νΈλ₯Ό μλ ₯νκ³  μμ± λ³ν λ²νΌμ ν΄λ¦­νμΈμ.</p>
      <div className="box">
        <input onChange={handleInput} value={value} />
        <button onClick={handleButton}>μμ± λ³ν</button>
      </div>
    </div>
  )
}

export default App
```

### :zap: getSpeech.ts
- `window.speechSynthesis.getVoices();` ν¨μλ₯Ό μ΄μ©νμ¬ λλ°μ΄μ€μ λ΄μ₯λ μμ±μ κ°μ Έμ¬ μ μλ€.
- `if μ‘°κ±΄μ`μ μ΄μ©νμ¬ νκ΅­μ΄ μμ±μ΄ μλμ§λ₯Ό νμΈνμ¬, νκ΅­μ΄κ° μλ€λ©΄ μμ±μ΄ λμ€κ²λ μ€μ νλ€.
```js
export const getSpeech = (text: any) => {
  let voices: any[] = [];

  //λλ°μ΄μ€μ λ΄μ₯λ voiceλ₯Ό κ°μ Έμ¨λ€.
  const setVoiceList = () => {
    voices = window.speechSynthesis.getVoices();
  };

  setVoiceList();

  if (window.speechSynthesis.onvoiceschanged !== undefined) {
    //voice listμ λ³κ²½λμλ, voiceλ₯Ό λ€μ κ°μ Έμ¨λ€.
    window.speechSynthesis.onvoiceschanged = setVoiceList;
  }

  const speech = (txt: string | undefined) => {
    const lang = "ko-KR";
    const utterThis = new SpeechSynthesisUtterance(txt);

    utterThis.lang = lang;

    /* 
      νκ΅­μ΄ vocie μ°ΎκΈ°
      λλ°μ΄μ€ λ³λ‘ νκ΅­μ΄λ ko-KR λλ ko_KRλ‘ voiceκ° μ μλμ΄ μλ€.
    */
    const kor_voice = voices.find(
      (elem) => elem.lang === lang || elem.lang === lang.replace("-", "_")
    );

    // νκ΅­μ΄ voiceκ° μλ€λ©΄ ? utteranceμ λͺ©μλ¦¬λ₯Ό μ€μ νλ€ : λ¦¬ν΄νμ¬ λͺ©μλ¦¬κ° λμ€μ§ μλλ‘ νλ€.
    if (kor_voice) {
      utterThis.voice = kor_voice;
    } else {
      return;
    }

    //utteranceλ₯Ό μ¬μ(speak)νλ€.
    window.speechSynthesis.speak(utterThis);
  };

  speech(text);
};
```

## :test_tube: μμ±λ³ν λ²νΌ ν΄λ¦­.
- μμ± λ³ν λ²νΌμ ν΄λ¦­νλ©΄ μΉνμ΄μ§μμ input μμ νμ€νΈ λ΄μ©μ μ½μ΄μ μμ± λ³νμ΄ λ©λλ€.
## π μΆμ²
- <a href="https://joylee-developer.tistory.com/35">Javascript30 - day23 Speech Synthesis</a>
- <a href="https://sub0709.tistory.com/86">[javascript] νλ¬κ·ΈμΈ μλ Text-To-Speech λ§λ€κΈ°</a>
