<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>테스트용 웹페이지</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0; font-family: Arial, "Noto Sans KR", sans-serif;
      background: #f3f5f9; color: #202536;
    }
    header {
      background: #293a80; color: white; padding: 22px 8%;
      display: flex; justify-content: space-between; align-items: center;
    }
    header h1 { margin: 0; font-size: 1.35rem; }
    nav a { color: white; text-decoration: none; margin-left: 20px; }
    main { width: min(900px, 90%); margin: 48px auto; }
    .hero {
      background: white; padding: 36px; border-radius: 18px;
      box-shadow: 0 8px 28px #1c2b5512; text-align: center;
    }
    .hero h2 { margin-top: 0; font-size: 2rem; }
    .hero p { color: #667085; line-height: 1.7; }
    button {
      border: 0; border-radius: 9px; padding: 12px 20px;
      background: #4056b4; color: white; font-size: 1rem; cursor: pointer;
    }
    button:hover { background: #293a80; }
    .counter { margin-top: 28px; padding: 22px; background: #eef1ff; border-radius: 12px; }
    #count { font-size: 2rem; font-weight: bold; margin: 12px; }
    .cards { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; margin-top: 22px; }
    .card { background: white; padding: 22px; border-radius: 14px; box-shadow: 0 5px 18px #1c2b550b; }
    .card h3 { margin-top: 0; }
    footer { text-align: center; color: #7b8190; padding: 28px; }
    @media (max-width: 650px) {
      header { align-items: flex-start; gap: 12px; flex-direction: column; }
      nav a { margin: 0 14px 0 0; }
      .cards { grid-template-columns: 1fr; }
      .hero { padding: 26px 18px; }
    }
  </style>
</head>
<body>
  <header>
    <h1>테스트 페이지</h1>
    <nav>
      <a href="#home">홈</a>
      <a href="#features">기능</a>
      <a href="#counter">카운터</a>
    </nav>
  </header>

  <main id="home">
    <section class="hero">
      <h2>안녕하세요! 👋</h2>
      <p>HTML, CSS, JavaScript로 만든 간단한 테스트용 웹페이지입니다.<br>
         화면 크기와 버튼 동작을 자유롭게 확인해 보세요.</p>
      <button id="helloButton">눌러 보기</button>
      <p id="message" aria-live="polite"></p>
    </section>

    <section class="counter" id="counter">
      <h2>클릭 카운터</h2>
      <p>버튼을 누르면 숫자가 바뀝니다.</p>
      <div id="count">0</div>
      <button id="plusButton">+1 증가</button>
      <button id="resetButton" style="background:#687083">초기화</button>
    </section>

    <section class="cards" id="features">
      <article class="card"><h3>HTML</h3><p>웹페이지의 구조를 만듭니다.</p></article>
      <article class="card"><h3>CSS</h3><p>색상, 여백, 배치 등 디자인을 담당합니다.</p></article>
      <article class="card"><h3>JavaScript</h3><p>버튼 클릭 같은 동작을 처리합니다.</p></article>
    </section>
  </main>
  <footer>나만의 테스트 페이지 · 2026</footer>

  <script>
    let count = 0;
    const countDisplay = document.querySelector("#count");

    document.querySelector("#helloButton").addEventListener("click", () => {
      document.querySelector("#message").textContent = "버튼이 정상적으로 작동합니다!";
    });

    document.querySelector("#plusButton").addEventListener("click", () => {
      count++;
      countDisplay.textContent = count;
    });

    document.querySelector("#resetButton").addEventListener("click", () => {
      count = 0;
      countDisplay.textContent = count;
    });
  </script>
</body>
</html>
