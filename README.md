# 🎙️ React 만든 Text To Speech 예제 파일입니다.
:octocat: https://light9639.github.io/React-TTS/

![light9639 github io_React-TTS_](https://user-images.githubusercontent.com/95972251/212852447-75ab10d7-7671-415d-8fad-2c04265ebfd5.png)

:sparkles: React 만든 Text To Speech 예제 파일입니다. :sparkles:
## :tada: React 생성
- React 생성
```bash
npm create-react-app my-app
# or
yarn create react-app my-app
```

- vite를 이용하여 프로젝트를 생성하려면
```bash
npm create vite@latest
# or
yarn create vite
```
- 터미널에서 실행 후 프로젝트 이름 만든 후 React 선택, Typescirpt 선택하면 생성 완료.

## ✒️ App.tsx, getSpeech.ts 수정 및 작성
### :zap: App.tsx
```js
import { useEffect, useState } from 'react'
import reactLogo from './assets/react.svg'
import './App.css';
import { getSpeech } from "./utils/getSpeech";

function App() {
  const [value, setValue] = useState("안녕하세요");

  //음성 변환 목소리 preload
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
      <h1>TTS(한국어)</h1>
      <p>텍스트를 입력하고 음성 변환 버튼을 클릭하세요.</p>
      <div className="box">
        <input onChange={handleInput} value={value} />
        <button onClick={handleButton}>음성 변환</button>
      </div>
    </div>
  )
}

export default App
```

### :zap: getSpeech.ts
```js
export const getSpeech = (text: any) => {
  let voices: any[] = [];

  //디바이스에 내장된 voice를 가져온다.
  const setVoiceList = () => {
    voices = window.speechSynthesis.getVoices();
  };

  setVoiceList();

  if (window.speechSynthesis.onvoiceschanged !== undefined) {
    //voice list에 변경됐을때, voice를 다시 가져온다.
    window.speechSynthesis.onvoiceschanged = setVoiceList;
  }

  const speech = (txt: string | undefined) => {
    const lang = "ko-KR";
    const utterThis = new SpeechSynthesisUtterance(txt);

    utterThis.lang = lang;

    /* 한국어 vocie 찾기
      디바이스 별로 한국어는 ko-KR 또는 ko_KR로 voice가 정의되어 있다.
    */
    const kor_voice = voices.find(
      (elem) => elem.lang === lang || elem.lang === lang.replace("-", "_")
    );

    //힌국어 voice가 있다면 ? utterance에 목소리를 설정한다 : 리턴하여 목소리가 나오지 않도록 한다.
    if (kor_voice) {
      utterThis.voice = kor_voice;
    } else {
      return;
    }

    //utterance를 재생(speak)한다.
    window.speechSynthesis.speak(utterThis);
  };

  speech(text);
};
```

## :test_tube: 음성변환 버튼 클릭.
- 음성 변환 버튼을 클릭하면 웹페이지에서 input 속의 텍스트 내용을 읽어서 음성 변환이 됩니다.
