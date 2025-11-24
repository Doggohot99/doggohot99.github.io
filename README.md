<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>성도고 테스트 챗봇</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: #0f172a;
      color: #e5e7eb;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }
    .container {
      width: 100%;
      max-width: 420px;
      background: #020617;
      border-radius: 18px;
      box-shadow: 0 18px 40px rgba(0,0,0,0.6);
      border: 1px solid rgba(148,163,184,0.25);
      overflow: hidden;
    }
    .header {
      padding: 14px 18px;
      border-bottom: 1px solid rgba(148,163,184,0.25);
      display: flex;
      align-items: center;
      gap: 10px;
    }
    .bot-avatar {
      width: 30px;
      height: 30px;
      border-radius: 50%;
      background: radial-gradient(circle at 30% 20%, #38bdf8, #6366f1);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 16px;
      font-weight: 700;
      color: white;
    }
    .title-block {
      display: flex;
      flex-direction: column;
      gap: 2px;
    }
    .title-block h1 {
      font-size: 15px;
      font-weight: 600;
    }
    .title-block span {
      font-size: 11px;
      color: #9ca3af;
    }
    .badge {
      margin-left: auto;
      font-size: 10px;
      padding: 3px 8px;
      border-radius: 999px;
      background: rgba(34,197,94,0.1);
      color: #4ade80;
      border: 1px solid rgba(74,222,128,0.4);
    }
    .chat-window {
      padding: 14px 14px 6px 14px;
      height: 420px;
      overflow-y: auto;
      background: radial-gradient(circle at top, #020617, #020617 40%, #020617 100%);
    }
    .msg {
      margin-bottom: 10px;
      display: flex;
      gap: 8px;
    }
    .msg.bot .bubble {
      background: #020617;
      border: 1px solid rgba(148,163,184,0.35);
      color: #e5e7eb;
      border-radius: 14px;
      border-top-left-radius: 4px;
    }
    .msg.user {
      justify-content: flex-end;
    }
    .msg.user .bubble {
      background: #4f46e5;
      color: white;
      border-radius: 14px;
      border-top-right-radius: 4px;
    }
    .bubble {
      max-width: 75%;
      padding: 8px 11px;
      font-size: 13px;
      line-height: 1.45;
    }
    .time {
      font-size: 10px;
      color: #6b7280;
      margin-top: 2px;
    }
    .input-bar {
      border-top: 1px solid rgba(148,163,184,0.3);
      padding: 8px;
      background: #020617;
      display: flex;
      align-items: center;
      gap: 6px;
    }
    .input-bar input {
      flex: 1;
      padding: 9px 10px;
      border-radius: 999px;
      border: 1px solid rgba(75,85,99,0.8);
      background: #020617;
      color: #e5e7eb;
      font-size: 13px;
      outline: none;
    }
    .input-bar input::placeholder {
      color: #6b7280;
    }
    .input-bar button {
      border: none;
      border-radius: 999px;
      padding: 8px 12px;
      font-size: 12px;
      font-weight: 500;
      cursor: pointer;
      background: linear-gradient(135deg,#38bdf8,#4f46e5);
      color: white;
    }
    .hint {
      font-size: 10px;
      color: #6b7280;
      text-align: center;
      padding-bottom: 6px;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <div class="bot-avatar">S</div>
      <div class="title-block">
        <h1>성도AI · 테스트 챗봇</h1>
        <span>성도고 위키 기반 도움봇 (테스트용)</span>
      </div>
      <div class="badge">BETA</div>
    </div>

    <div class="chat-window" id="chat">
      <div class="msg bot">
        <div class="bubble">
          안녕! 👋<br>
          여기는 <b>성도고 테스트 챗봇</b>이야.<br>
          아래 입력창에 아무 말이나 써보고, 진짜 챗봇 붙이기 전에
          UI만 먼저 테스트해볼 수 있어.
          <div class="time">지금은 더미 챗봇이라 실제 답변 로직은 없음</div>
        </div>
      </div>

      <div class="msg user">
        <div class="bubble">
          시험 기간 일정 같은 것도 나중에 여기서 물어볼 수 있는 거야?
          <div class="time">YOU · just now</div>
        </div>
      </div>

      <div class="msg bot">
        <div class="bubble">
          그렇지! 나중에 성도위키/노션 내용을 연결하면<br>
          &lt;시험 일정, 수행 비율, 규정&gt; 같은 걸 여기서 바로 물어볼 수 있게 만들 거야.
        </div>
      </div>
    </div>

    <div class="hint">
      🔧 지금은 프론트만 있는 테스트 버전입니다. 나중에 여기에 AI 스크립트를 붙이면 진짜 챗봇이 됩니다.
    </div>

    <div class="input-bar">
      <input id="userInput" type="text" placeholder="테스트용으로 아무 말이나 입력해봐 (엔터도 가능)" />
      <button id="sendBtn">전송</button>
    </div>
  </div>

  <script>
    // 아주 간단한 프론트 테스트용 JS (실제 AI랑 연결 X)
    const chat = document.getElementById('chat');
    const input = document.getElementById('userInput');
    const sendBtn = document.getElementById('sendBtn');

    function addMessage(text, type = 'user') {
      const msg = document.createElement('div');
      msg.className = 'msg ' + type;

      const bubble = document.createElement('div');
      bubble.className = 'bubble';
      bubble.innerHTML = text;

      msg.appendChild(bubble);
      chat.appendChild(msg);
      chat.scrollTop = chat.scrollHeight;
    }

    function handleSend() {
      const text = input.value.trim();
      if (!text) return;
      addMessage(text, 'user');

      // 실제 AI 대신, 더미 응답
      setTimeout(() => {
        addMessage('⚙️ [테스트 봇] 지금은 더미 챗봇이야!<br>나중에 여기에 Chatling / Flowise 스크립트를 붙이면 진짜로 답변할 수 있어.', 'bot');
      }, 400);

      input.value = '';
    }

    sendBtn.addEventListener('click', handleSend);
    input.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') handleSend();
    });

    // 🔻 나중에 Chatling 같은 외부 AI 위젯을 붙이고 싶으면
    // 이 아래에 제공된 <script> 코드를 추가하면 됨.
    //
    // 예시)
    // <script src="https://js.chatling.ai/v1/chatling.js"
    //   data-chatbot-id="여기에_너_챗봇_ID"></script>
  </script>
</body>
</html>
