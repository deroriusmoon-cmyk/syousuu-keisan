<!doctype html>
<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>小数の筆算練習</title>

<style>
  :root {
    --blue: #2457a5;
    --green: #167347;
    --red: #b32635;
    --yellow: #fff0a6;
    --cell: 48px;
  }

  * {
    box-sizing: border-box;
  }

  [hidden] {
    display: none !important;
  }

  body {
    margin: 0;
    background: #e8edf3;
    color: #20242b;
    font-family:
      "Yu Gothic",
      "Hiragino Kaku Gothic ProN",
      system-ui,
      sans-serif;
    font-size: 18px;
  }

  main {
    width: min(100%, 1400px);
    min-height: 100vh;
    margin: auto;
    padding: 24px;
    background: white;
  }

  h1 {
    margin: 0 0 10px;
    font-size: clamp(26px, 2.5vw, 36px);
  }

  h2 {
    margin: 0 0 15px;
    font-size: clamp(23px, 2vw, 30px);
  }

  button {
    min-height: 52px;
    padding: 10px 16px;
    border: 2px solid var(--blue);
    border-radius: 9px;
    background: white;
    color: var(--blue);
    font: inherit;
    font-weight: 600;
    cursor: pointer;
    touch-action: manipulation;
  }

  button.primary {
    background: var(--blue);
    color: white;
  }

  button:disabled {
    opacity: .45;
    cursor: default;
  }

  .menu {
    max-width: 850px;
    margin: 5vh auto;
  }

  .menu p,
  .note {
    line-height: 1.65;
  }

  .menu-buttons,
  .toolbar,
  .controls {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    gap: 11px;
  }

  .menu-buttons {
    margin: 14px 0 29px;
  }

  .toolbar {
    justify-content: space-between;
    padding-bottom: 16px;
    border-bottom: 3px solid #252525;
  }

  .toolbar h1 {
    margin-bottom: 5px;
  }

  .timer {
    color: var(--blue);
    font-size: 28px;
    font-weight: 800;
    font-variant-numeric: tabular-nums;
  }

  .layout {
    display: grid;
    grid-template-columns: minmax(220px, 300px) minmax(0, 1fr);
    gap: 20px;
    margin-top: 20px;
  }

  .question-list {
    display: grid;
    align-content: start;
    gap: 11px;
  }

  .question {
    width: 100%;
    min-height: 93px;
    padding: 13px;
    border: 1px solid #aebbd0;
    border-radius: 5px;
    color: #222;
    text-align: left;
    font-weight: normal;
  }

  .question-title {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 6px;
    font-size: 20px;
    font-weight: 700;
    line-height: 1.3;
  }

  /* ★小問の番号を四角で囲む */
  .question-number {
    display: inline-flex;
    flex: 0 0 auto;
    align-items: center;
    justify-content: center;
    width: 1.65em;
    height: 1.65em;
    border: 2px solid currentColor;
    border-radius: 2px;
    font-size: 0.9em;
    line-height: 1;
    font-variant-numeric: tabular-nums;
  }

  .question.selected {
    border: 3px solid var(--blue);
    padding: 11px;
    background: #eef4ff;
  }

  .question.done {
    background: #e7f7ec;
  }

  .question .progress {
    font-size: 15px;
  }

  .work {
    min-width: 0;
    padding: clamp(16px, 2vw, 28px);
    border: 3px solid var(--blue);
    border-radius: 5px;
  }

  .work-heading {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .original-problem {
    margin: 0 0 14px;
    font-size: clamp(24px, 2.2vw, 32px);
    font-weight: 800;
  }

  .note {
    margin: 12px 0;
    padding: 12px 15px;
    background: #edf4ff;
  }

  .hissan-scroll {
    overflow-x: auto;
    padding: 15px 0 20px;
  }

  .hissan {
    width: max-content;
    border-collapse: collapse;
    font-family: "Courier New", monospace;
    font-variant-numeric: tabular-nums;
  }

  .hissan td {
    position: relative;
    width: var(--cell);
    min-width: var(--cell);
    height: 53px;
    padding: 0;
    text-align: center;
    font-size: 30px;
  }

  .hissan td.label {
    width: 112px;
    min-width: 112px;
    padding-right: 10px;
    text-align: right;
    font-family: sans-serif;
    font-size: 20px;
    white-space: nowrap;
  }

  .hissan tr.line td.digit {
    border-bottom: 3px solid #222;
  }

  .hissan tr.bracket td.digit {
    border-top: 3px solid #222;
  }

  .hissan tr.bracket td.first {
    border-left: 3px solid #222;
  }

  .hissan td.active {
    background: var(--yellow);
    box-shadow: inset 0 0 0 2px #d4a535;
  }

  .hissan td.checked {
    color: var(--green);
  }

  .hissan td.faint {
    color: #9ba6b5;
  }

  .hissan td.decimal-after::after {
    content: ".";
    position: absolute;
    right: -7px;
    bottom: 4px;
    z-index: 2;
    color: var(--red);
    font-weight: bold;
  }

  .instruction {
    margin: 13px 0;
    font-size: 21px;
    font-weight: bold;
    line-height: 1.6;
  }

  .shift-counter {
    margin: 8px 0 14px;
    color: var(--blue);
    font-weight: bold;
    line-height: 1.6;
  }

  .answer-area {
    margin: 8px 0 20px;
    font-size: 24px;
  }

  .answer-box {
    display: inline-block;
    min-width: 160px;
    min-height: 46px;
    padding: 4px 12px;
    border-bottom: 3px solid #222;
    font-family: "Courier New", monospace;
    font-variant-numeric: tabular-nums;
  }

  .answer-box.active {
    background: var(--yellow);
    outline: 2px solid #d4a535;
  }

  .keypad {
    display: grid;
    grid-template-columns: repeat(3, 77px);
    gap: 9px;
    width: max-content;
    margin: 13px 0;
  }

  .keypad button {
    width: 77px;
    height: 63px;
    padding: 0;
    background: #f2f6fd;
    font-size: 26px;
    font-weight: normal;
  }

  .feedback {
    min-height: 1.7em;
    font-size: 19px;
    font-weight: bold;
    line-height: 1.5;
  }

  .feedback.ok {
    color: var(--green);
  }

  .feedback.ng {
    color: var(--red);
  }

  .bottom-controls {
    margin-top: 19px;
  }

  /* iPad横画面 */
  @media (min-width: 760px) and (max-width: 1100px) {
    main {
      padding: 16px;
    }

    .layout {
      grid-template-columns: minmax(200px, 25%) minmax(0, 1fr);
      gap: 15px;
    }

    .work {
      padding: 16px;
    }

    :root {
      --cell: 42px;
    }

    .hissan td {
      height: 49px;
      font-size: 27px;
    }

    .hissan td.label {
      width: 98px;
      min-width: 98px;
      font-size: 18px;
    }
  }

  /* 縦画面・小さい画面 */
  @media (max-width: 759px) {
    main {
      padding: 12px;
    }

    .layout {
      grid-template-columns: 1fr;
    }

    .question-list {
      grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    .question {
      min-height: 83px;
      padding: 9px;
    }

    .question-title {
      gap: 6px;
      font-size: 16px;
    }

    .question .progress {
      font-size: 14px;
    }

    .work {
      padding: 12px;
    }

    :root {
      --cell: 38px;
    }

    .hissan td {
      height: 44px;
      font-size: 24px;
    }

    .hissan td.label {
      width: 86px;
      min-width: 86px;
      font-size: 16px;
    }
  }
</style>
</head>

<body>
<main>
  <section id="menu" class="menu">
    <h1>小数の筆算練習</h1>

    <p>
      かけ算は数字の部分を先に筆算し、最後に答えの小数点を動かします。
      わり算は筆算を始める前に、両方の小数点を動かします。
    </p>

    <h2>① 計算を選ぶ</h2>
    <div class="menu-buttons">
      <button id="multiplyChoice" class="primary">
        小数のかけ算
      </button>
      <button id="divideChoice">
        小数のわり算
      </button>
    </div>

    <h2>② 練習方法を選ぶ</h2>
    <div class="menu-buttons">
      <button id="practiceStart" class="primary">
        通常練習：6問
      </button>
      <button id="timedStart">
        タイムアタック：3分間
      </button>
    </div>
  </section>

  <section id="app" hidden>
    <div class="toolbar">
      <div>
        <h1 id="title"></h1>
        <div id="status"></div>
      </div>

      <div id="timer" class="timer"></div>
      <button id="back">メニューへ</button>
    </div>

    <div class="layout">
      <nav
        id="questionList"
        class="question-list"
        aria-label="問題一覧"
      ></nav>

      <section class="work">
        <div id="workContents"></div>
      </section>
    </div>

    <div class="controls bottom-controls">
      <button id="newQuestions">新しい問題</button>
    </div>
  </section>
</main>

<script>
(() => {
  "use strict";

  const $ = id => document.getElementById(id);

  const SHEET_SIZE = 6;
  const TIME_LIMIT_MS = 3 * 60 * 1000;
  const DIVISION_COLUMNS = 7;
  const MULTIPLICATION_COLUMNS = 7;
  const DIVISION_BASE_COLUMN = 2;

  let operation = "multiply";
  let mode = "practice";
  let questions = [];
  let selected = 0;
  let score = 0;
  let finished = false;
  let deadline = 0;
  let timerHandle = null;
  let feedback = { message: "", kind: "" };

  function random(min, max) {
    return Math.floor(
      Math.random() * (max - min + 1)
    ) + min;
  }

  /*
    問題の数は100倍した整数で保持する。
    40 → 0.4、25 → 0.25、1 → 0.01。
  */
  function shownNumber(n) {
    return (n / 100)
      .toFixed(2)
      .replace(/0+$/, "")
      .replace(/\.$/, "");
  }

  function decimalPlaces(n) {
    return shownNumber(n).split(".")[1]?.length ?? 0;
  }

  function digitsAsInteger(n) {
    return n / (10 ** (2 - decimalPlaces(n)));
  }

  function pointMovedLeft(integer, places) {
    if (places === 0) {
      return String(integer);
    }

    const digits = String(integer).padStart(
      places + 1,
      "0"
    );

    return (
      digits.slice(0, -places) +
      "." +
      digits.slice(-places)
    );
  }

  function exactDecimal(scaledInteger, places) {
    const text = pointMovedLeft(
      scaledInteger,
      places
    );

    if (!text.includes(".")) {
      return text;
    }

    return text
      .replace(/0+$/, "")
      .replace(/\.$/, "");
  }

  /*
    わり算で小数点を動かす途中の表示。
    例：0.34 ÷ 0.8
        3.4 ÷ 8.0
         34 ÷ 80
  */
  function movedDivisionText(n, shift, totalShifts) {
    if (shift === 0) {
      return shownNumber(n);
    }

    const scaled = n * (10 ** shift);
    const remaining = totalShifts - shift;

    if (remaining === 0) {
      return String(scaled / 100);
    }

    return (scaled / 100).toFixed(remaining);
  }

  /* 小数第4位までで割り切れるわり算の候補 */
  const divisionCandidates = [];
  const mixedDivisionCandidates = [];

  for (let a = 1; a <= 100; a++) {
    for (let b = 1; b <= 100; b++) {
      if (
        (a * 10000) % b === 0 &&
        a <= b * 9
      ) {
        const pair = [a, b];
        divisionCandidates.push(pair);

        if (
          decimalPlaces(a) !== decimalPlaces(b)
        ) {
          mixedDivisionCandidates.push(pair);
        }
      }
    }
  }

  function generateNumbers(index) {
    // 最初の問題は動作を確かめやすい例にする。
    if (index === 0 && operation === "multiply") {
      return [40, 25]; // 0.4 × 0.25
    }

    if (index === 0 && operation === "divide") {
      return [40, 20]; // 0.4 ÷ 0.2
    }

    if (operation === "multiply") {
      // 一部の問題は小数の桁数を必ず異ならせる。
      if (index % 2 === 0) {
        const onePlace = random(1, 9) * 10;
        let twoPlaces;

        do {
          twoPlaces = random(1, 99);
        } while (
          decimalPlaces(twoPlaces) !== 2
        );

        return Math.random() < 0.5
          ? [onePlace, twoPlaces]
          : [twoPlaces, onePlace];
      }

      return [
        random(1, 100),
        random(1, 100)
      ];
    }

    const pool = index % 2 === 0
      ? mixedDivisionCandidates
      : divisionCandidates;

    return pool[random(0, pool.length - 1)];
  }

  function makeQuestion(index) {
    const [a, b] = generateNumbers(index);
    const tasks = [];
    const steps = [];

    let integerProduct = null;
    let answerPlaces = null;
    let requiredShifts = 0;

    if (operation === "multiply") {
      const topInteger = digitsAsInteger(a);
      const bottomInteger = digitsAsInteger(b);

      integerProduct =
        topInteger * bottomInteger;

      answerPlaces =
        decimalPlaces(a) +
        decimalPlaces(b);

      const bottomDigits = String(bottomInteger)
        .split("")
        .reverse()
        .map(Number);

      bottomDigits.forEach((digit, place) => {
        tasks.push({
          type: "partial",
          place,
          expected: String(
            topInteger * digit
          ),
          instruction:
            `${place + 1}段目：小数点をいったん考えず、` +
            `${topInteger} × ${digit} を黄色いマスに入力`
        });
      });

      if (bottomDigits.length > 1) {
        tasks.push({
          type: "sum",
          expected: String(integerProduct),
          instruction:
            "部分積を位をそろえて足し、整数の合計を入力"
        });
      }

      tasks.push({
        type: "answerShift",
        expected: exactDecimal(a * b, 4),
        instruction:
          "整数の積の小数点を左へ動かして答えを作る"
      });

    } else {
      requiredShifts = Math.max(
        decimalPlaces(a),
        decimalPlaces(b)
      );

      const factor = 10 ** requiredShifts;
      const dividend = (a * factor) / 100;
      const divisor = (b * factor) / 100;

      let partial = dividend;
      let remainder = 0;
      let place = 0;

      do {
        const digit = Math.floor(
          partial / divisor
        );

        const product = digit * divisor;
        remainder = partial - product;

        const step = {
          place,
          partial,
          digit,
          product,
          remainder,
          quotientTask: tasks.length,
          productTask: null,
          remainderTask: null
        };

        const placeName = place === 0
          ? "整数部分"
          : `小数第${place}位`;

        tasks.push({
          type: "quotient",
          step: place,
          expected: String(digit),
          instruction:
            `${placeName}の商を、上の黄色いマスに入力`
        });

        // 商が0なら「ひく」「余り」を飛ばす。
        if (digit !== 0) {
          step.productTask = tasks.length;

          tasks.push({
            type: "product",
            step: place,
            expected: String(product),
            instruction:
              `${divisor} × ${digit} を「ひく」の段に入力`
          });

          step.remainderTask = tasks.length;

          tasks.push({
            type: "remainder",
            step: place,
            expected: String(remainder),
            instruction:
              `${partial} − ${product} の余りを入力`
          });
        }

        steps.push(step);
        partial = remainder * 10;
        place++;

      } while (
        remainder !== 0 &&
        place <= 4
      );

      tasks.push({
        type: "answer",
        expected: exactDecimal(
          (a * 10000) / b,
          4
        ),
        instruction:
          "上に並んだ商を読んで、答えを入力"
      });
    }

    return {
      a,
      b,
      tasks,
      steps,
      integerProduct,
      answerPlaces,
      requiredShifts,
      divisionShift: 0,
      answerShift: 0,
      progress: 0,
      input: "",
      done: false
    };
  }

  function createSheet() {
    questions = Array.from(
      { length: SHEET_SIZE },
      (_, index) => makeQuestion(index)
    );

    selected = 0;
    feedback = { message: "", kind: "" };
    render();
  }

  function start(newMode) {
    mode = newMode;
    score = 0;
    finished = false;

    $("menu").hidden = true;
    $("app").hidden = false;

    $("title").textContent =
      operation === "divide"
        ? "小数のわり算・筆算"
        : "小数のかけ算・筆算";

    clearInterval(timerHandle);

    if (mode === "timed") {
      deadline = Date.now() + TIME_LIMIT_MS;
      timerHandle = setInterval(
        updateTimer,
        100
      );
    }

    createSheet();
    updateTimer();
  }

  function finishTimed() {
    finished = true;
    clearInterval(timerHandle);
    render();
  }

  function updateTimer() {
    if (mode !== "timed") {
      $("timer").textContent = "";
      return;
    }

    const left = Math.max(
      0,
      Math.ceil(
        (deadline - Date.now()) / 1000
      )
    );

    const minutes = Math.floor(left / 60);
    const seconds = String(
      left % 60
    ).padStart(2, "0");

    $("timer").textContent =
      `残り ${minutes}:${seconds}`;

    if (left === 0 && !finished) {
      finishTimed();
    }
  }

  function timeIsUp() {
    if (
      mode === "timed" &&
      Date.now() >= deadline
    ) {
      if (!finished) {
        finishTimed();
      }

      return true;
    }

    return false;
  }

  function addText(
    tag,
    className,
    text,
    parent
  ) {
    const element =
      document.createElement(tag);

    element.className = className;
    element.textContent = text;
    parent.append(element);

    return element;
  }

  function makeControl(
    parent,
    text,
    onClick,
    options = {}
  ) {
    const button =
      document.createElement("button");

    button.type = "button";
    button.textContent = text;

    if (options.primary) {
      button.classList.add("primary");
    }

    if (options.disabled) {
      button.disabled = true;
    }

    button.addEventListener(
      "click",
      onClick
    );

    parent.append(button);
    return button;
  }

  /* □で囲んだ番号を作る */
  function makeQuestionNumber(number) {
    const span =
      document.createElement("span");

    span.className = "question-number";
    span.textContent = String(number);

    return span;
  }

  /*
    筆算の1行を作る。
    digits は {列番号: 表示する数字}。
  */
  function makeRow(table, options = {}) {
    const {
      label = "",
      digits = {},
      inputRange = [],
      checked = false,
      faint = [],
      line = false,
      bracket = false,
      decimalAfter = -1
    } = options;

    const row =
      document.createElement("tr");

    if (line) {
      row.classList.add("line");
    }

    if (bracket) {
      row.classList.add("bracket");
    }

    const labelCell =
      document.createElement("td");

    labelCell.className = "label";
    labelCell.textContent = label;
    row.append(labelCell);

    const count = operation === "divide"
      ? DIVISION_COLUMNS
      : MULTIPLICATION_COLUMNS;

    for (let col = 0; col < count; col++) {
      const cell =
        document.createElement("td");

      cell.className = "digit";
      cell.textContent =
        digits[col] ?? "";

      if (inputRange.includes(col)) {
        cell.classList.add("active");
      }

      if (
        checked &&
        digits[col] !== undefined
      ) {
        cell.classList.add("checked");
      }

      if (faint.includes(col)) {
        cell.classList.add("faint");
      }

      if (decimalAfter === col) {
        cell.classList.add(
          "decimal-after"
        );
      }

      if (bracket && col === 0) {
        cell.classList.add("first");
      }

      row.append(cell);
    }

    table.append(row);
  }

  function positionedDigits(
    text,
    endingColumn
  ) {
    const result = {};
    const chars = [...String(text)];

    chars.forEach(
      (character, index) => {
        const col =
          endingColumn -
          chars.length +
          1 +
          index;

        if (col >= 0) {
          result[col] = character;
        }
      }
    );

    return result;
  }

  function positionedDecimal(
    text,
    endingColumn
  ) {
    const parts =
      String(text).split(".");

    const integerPart = parts[0];
    const fractionPart = parts[1];

    const allDigits =
      integerPart +
      (fractionPart ?? "");

    const digits = positionedDigits(
      allDigits,
      endingColumn
    );

    let decimalAfter = -1;

    if (fractionPart !== undefined) {
      const startColumn =
        endingColumn -
        allDigits.length +
        1;

      decimalAfter =
        startColumn +
        integerPart.length -
        1;
    }

    return {
      digits,
      decimalAfter
    };
  }

  function activeDigits(
    q,
    endingColumn,
    width
  ) {
    const start = Math.max(
      0,
      endingColumn - width + 1
    );

    const inputRange = [];

    for (
      let col = start;
      col <= endingColumn;
      col++
    ) {
      inputRange.push(col);
    }

    return {
      inputRange,
      digits: positionedDigits(
        q.input,
        endingColumn
      )
    };
  }

  function drawMultiplication(
    table,
    q
  ) {
    // 元の小数を表示したまま計算する。
    makeRow(table, {
      label: "上",
      ...positionedDecimal(
        shownNumber(q.a),
        6
      )
    });

    makeRow(table, {
      label: "× 下",
      ...positionedDecimal(
        shownNumber(q.b),
        6
      ),
      line: true
    });

    const bottomInteger =
      digitsAsInteger(q.b);

    const partialCount =
      String(bottomInteger).length;

    for (
      let index = 0;
      index < partialCount;
      index++
    ) {
      if (q.progress < index) {
        continue;
      }

      const ending = 6 - index;
      const current =
        q.progress === index;

      makeRow(table, {
        label: `${index + 1}段目`,

        ...(current
          ? activeDigits(
              q,
              ending,
              3
            )
          : {
              digits:
                positionedDigits(
                  q.tasks[index]
                    .expected,
                  ending
                ),
              checked: true
            }),

        line:
          index ===
          partialCount - 1
      });
    }

    if (partialCount > 1) {
      const sumIndex =
        partialCount;

      if (q.progress >= sumIndex) {
        const current =
          q.progress ===
          sumIndex;

        makeRow(table, {
          label: "合計",

          ...(current
            ? activeDigits(
                q,
                6,
                5
              )
            : {
                digits:
                  positionedDigits(
                    q.tasks[sumIndex]
                      .expected,
                    6
                  ),
                checked: true
              })
        });
      }
    }
  }

  function drawDivision(
    table,
    q
  ) {
    const shifting =
      q.divisionShift <
      q.requiredShifts;

    const quotientDigits = {};
    let activeQuotientColumn = -1;

    if (!shifting) {
      q.steps.forEach(
        (step, index) => {
          const col =
            DIVISION_BASE_COLUMN +
            index;

          if (
            q.progress >
            step.quotientTask
          ) {
            quotientDigits[col] =
              String(step.digit);

          } else if (
            q.progress ===
            step.quotientTask
          ) {
            activeQuotientColumn =
              col;

            if (q.input !== "") {
              quotientDigits[col] =
                q.input;
            }
          }
        }
      );
    }

    const showQuotientDecimal =
      !shifting &&
      q.steps.length > 1 &&
      q.progress >
        q.steps[0].quotientTask;

    makeRow(table, {
      label: "商",
      digits: quotientDigits,

      inputRange:
        activeQuotientColumn >= 0
          ? [
              activeQuotientColumn
            ]
          : [],

      decimalAfter:
        showQuotientDecimal
          ? DIVISION_BASE_COLUMN
          : -1
    });

    const divisorText = shifting
      ? movedDivisionText(
          q.b,
          q.divisionShift,
          q.requiredShifts
        )
      : String(
          (
            q.b *
            (
              10 **
              q.requiredShifts
            )
          ) / 100
        );

    const dividendText = shifting
      ? movedDivisionText(
          q.a,
          q.divisionShift,
          q.requiredShifts
        )
      : String(
          (
            q.a *
            (
              10 **
              q.requiredShifts
            )
          ) / 100
        );

    const placedDividend =
      positionedDecimal(
        dividendText,
        DIVISION_BASE_COLUMN
      );

    const dividendDigits = {
      ...placedDividend.digits
    };

    const addedZeroColumns = [];

    if (!shifting) {
      for (
        let index = 1;
        index < q.steps.length;
        index++
      ) {
        if (
          q.progress >=
          q.steps[index]
            .quotientTask
        ) {
          const col =
            DIVISION_BASE_COLUMN +
            index;

          dividendDigits[col] =
            "0";

          addedZeroColumns.push(
            col
          );
        }
      }
    }

    makeRow(table, {
      label: `${divisorText} ）`,
      digits: dividendDigits,
      bracket: true,

      decimalAfter:
        placedDividend
          .decimalAfter,

      faint:
        addedZeroColumns
    });

    if (shifting) {
      return;
    }

    q.steps.forEach(
      (step, index) => {
        if (
          q.progress <
          step.quotientTask
        ) {
          return;
        }

        const ending =
          DIVISION_BASE_COLUMN +
          index;

        if (index > 0) {
          const downDigits =
            positionedDigits(
              step.partial,
              ending
            );

          makeRow(table, {
            label: "↓",
            digits: downDigits,
            faint:
              Object.keys(
                downDigits
              ).map(Number)
          });
        }

        // 商が0なら、ひく・余りの段は表示しない。
        if (
          step.productTask !== null &&
          q.progress >=
            step.productTask
        ) {
          const current =
            q.progress ===
            step.productTask;

          makeRow(table, {
            label: "ひく",

            ...(current
              ? activeDigits(
                  q,
                  ending,
                  3
                )
              : {
                  digits:
                    positionedDigits(
                      step.product,
                      ending
                    ),
                  checked: true
                }),

            line: true
          });
        }

        if (
          step.remainderTask !== null &&
          q.progress >=
            step.remainderTask
        ) {
          const current =
            q.progress ===
            step.remainderTask;

          makeRow(table, {
            label: "余り",

            ...(current
              ? activeDigits(
                  q,
                  ending,
                  3
                )
              : {
                  digits:
                    positionedDigits(
                      step.remainder,
                      ending
                    ),
                  checked: true
                })
          });
        }
      }
    );
  }

  function renderQuestionList() {
    const list =
      $("questionList");

    list.replaceChildren();

    questions.forEach(
      (q, index) => {
        const button =
          document.createElement(
            "button"
          );

        button.className =
          "question" +
          (
            index === selected
              ? " selected"
              : ""
          ) +
          (
            q.done
              ? " done"
              : ""
          );

        const sign =
          operation === "divide"
            ? "÷"
            : "×";

        /* ★問題一覧の番号を四角で囲む */
        const title =
          document.createElement(
            "span"
          );

        title.className =
          "question-title";

        title.append(
          makeQuestionNumber(
            index + 1
          )
        );

        title.append(
          document.createTextNode(
            `${shownNumber(q.a)} ` +
            `${sign} ` +
            `${shownNumber(q.b)}`
          )
        );

        button.append(title);

        let progressText;

        if (q.done) {
          progressText =
            `答え：${
              q.tasks.at(-1)
                .expected
            }　✓`;

        } else if (
          operation === "divide" &&
          q.divisionShift <
            q.requiredShifts
        ) {
          progressText =
            `小数点：${
              q.divisionShift
            } / ${
              q.requiredShifts
            }桁`;

        } else if (
          operation === "multiply" &&
          q.tasks[
            q.progress
          ]?.type ===
            "answerShift"
        ) {
          progressText =
            `答えの小数点：${
              q.answerShift
            } / ${
              q.answerPlaces
            }桁`;

        } else {
          progressText =
            `筆算：${
              q.progress
            } / ${
              q.tasks.length
            }段階`;
        }

        addText(
          "span",
          "progress",
          progressText,
          button
        );

        button.addEventListener(
          "click",
          () => {
            if (
              finished ||
              timeIsUp()
            ) {
              return;
            }

            selected = index;
            feedback = {
              message: "",
              kind: ""
            };

            render();
          }
        );

        list.append(button);
      }
    );
  }

  function render() {
    $("status").textContent =
      mode === "timed"
        ? `タイムアタック｜正解 ${score}問`
        : `通常練習｜正解 ${score} / ${SHEET_SIZE}問`;

    $("newQuestions").hidden =
      mode === "timed" ||
      finished;

    renderQuestionList();

    const work =
      $("workContents");

    work.replaceChildren();

    if (finished) {
      addText(
        "h2",
        "",
        "終了！",
        work
      );

      addText(
        "p",
        "",
        `3分間で ${score}問正解しました。`,
        work
      );

      return;
    }

    const q =
      questions[selected];

    const divisionShifting =
      operation === "divide" &&
      q.divisionShift <
        q.requiredShifts;

    /* ★右側の問題番号も四角で囲む */
    const heading =
      document.createElement(
        "h2"
      );

    heading.className =
      "work-heading";

    heading.append(
      document.createTextNode(
        "問題"
      )
    );

    heading.append(
      makeQuestionNumber(
        selected + 1
      )
    );

    work.append(heading);

    addText(
      "p",
      "original-problem",
      `${shownNumber(q.a)} ` +
      `${
        operation === "divide"
          ? "÷"
          : "×"
      } ` +
      `${shownNumber(q.b)}`,
      work
    );

    let note;

    if (divisionShifting) {
      note =
        "下の筆算の図で、わる数とわられる数の小数点を" +
        "同じ回数だけ右へ動かしましょう。";

    } else if (
      operation === "divide"
    ) {
      note =
        "小数点を動かした後の数で筆算します。" +
        "商が0の位は、引き算を飛ばします。";

    } else {
      note =
        "元の小数を表示したまま、数字の部分を筆算します。" +
        "答えの小数点は最後に動かします。";
    }

    addText(
      "p",
      "note",
      note,
      work
    );

    const scroll = addText(
      "div",
      "hissan-scroll",
      "",
      work
    );

    const table =
      document.createElement(
        "table"
      );

    table.className =
      "hissan";

    table.setAttribute(
      "aria-label",
      "小数の筆算"
    );

    scroll.append(table);

    if (
      operation === "divide"
    ) {
      drawDivision(table, q);
    } else {
      drawMultiplication(
        table,
        q
      );
    }

    const answerArea =
      addText(
        "div",
        "answer-area",
        "",
        work
      );

    addText(
      "span",
      "",
      "答え：",
      answerArea
    );

    const answerBox =
      addText(
        "span",
        "answer-box",
        "",
        answerArea
      );

    const currentTask =
      q.tasks[q.progress];

    if (q.done) {
      answerBox.textContent =
        q.tasks.at(-1)
          .expected;

    } else if (
      operation === "multiply" &&
      currentTask?.type ===
        "answerShift"
    ) {
      answerBox.classList.add(
        "active"
      );

      answerBox.textContent =
        pointMovedLeft(
          q.integerProduct,
          q.answerShift
        );

    } else if (
      operation === "divide" &&
      currentTask?.type ===
        "answer"
    ) {
      answerBox.classList.add(
        "active"
      );

      answerBox.textContent =
        q.input;
    }

    if (q.done) {
      addText(
        "p",
        "feedback ok",
        "正解！ 次の問題を選んでください。",
        work
      );

      return;
    }

    if (divisionShifting) {
      addText(
        "p",
        "instruction",
        "この筆算の中で、両方の小数点を右へ動かそう",
        work
      );

      addText(
        "p",
        "shift-counter",
        `${q.divisionShift} / ` +
        `${q.requiredShifts}桁 移動しました`,
        work
      );

      const controls =
        addText(
          "div",
          "controls",
          "",
          work
        );

      makeControl(
        controls,
        "← 1桁戻す",
        () => moveDivision(-1),
        {
          disabled:
            q.divisionShift === 0
        }
      );

      makeControl(
        controls,
        "両方を右へ1桁 →",
        () => moveDivision(1),
        {
          primary: true
        }
      );

      addText(
        "p",
        `feedback ${
          feedback.kind
        }`,
        feedback.message,
        work
      );

      return;
    }

    if (
      operation === "multiply" &&
      currentTask?.type ===
        "answerShift"
    ) {
      addText(
        "p",
        "instruction",
        "整数の積の小数点を左へ動かして、答えを作ろう",
        work
      );

      addText(
        "p",
        "shift-counter",
        `左へ ${
          q.answerShift
        }桁 移動しました。` +
        "元の2つの数の小数部分は、合計で何桁かな？",
        work
      );

      const controls =
        addText(
          "div",
          "controls",
          "",
          work
        );

      makeControl(
        controls,
        "← 小数点を左へ1桁",
        () => moveAnswerPoint(1),
        {
          primary: true,
          disabled:
            q.answerShift >= 4
        }
      );

      makeControl(
        controls,
        "小数点を右へ1桁 →",
        () => moveAnswerPoint(-1),
        {
          disabled:
            q.answerShift === 0
        }
      );

      makeControl(
        controls,
        "答えを確認",
        confirmMultiplicationAnswer
      );

      addText(
        "p",
        `feedback ${
          feedback.kind
        }`,
        feedback.message,
        work
      );

      return;
    }

    addText(
      "p",
      "instruction",
      currentTask.instruction,
      work
    );

    const keypad =
      addText(
        "div",
        "keypad",
        "",
        work
      );

    [
      "7", "8", "9",
      "4", "5", "6",
      "1", "2", "3",
      ".", "0", "⌫"
    ].forEach(key => {
      makeControl(
        keypad,
        key,
        () => enterKey(key),
        {
          disabled:
            key === "." &&
            currentTask.type !==
              "answer"
        }
      );
    });

    const controls =
      addText(
        "div",
        "controls",
        "",
        work
      );

    makeControl(
      controls,
      "この段を確認",
      checkCurrent,
      {
        primary: true
      }
    );

    makeControl(
      controls,
      "入力を消す",
      () => {
        q.input = "";

        feedback = {
          message: "",
          kind: ""
        };

        render();
      }
    );

    addText(
      "p",
      `feedback ${
        feedback.kind
      }`,
      feedback.message,
      work
    );
  }

  function moveDivision(direction) {
    if (
      finished ||
      timeIsUp()
    ) {
      return;
    }

    const q =
      questions[selected];

    if (
      !q ||
      operation !== "divide" ||
      q.divisionShift >=
        q.requiredShifts
    ) {
      return;
    }

    const next =
      q.divisionShift +
      direction;

    if (
      next < 0 ||
      next >
        q.requiredShifts
    ) {
      return;
    }

    q.divisionShift = next;

    feedback =
      q.divisionShift ===
      q.requiredShifts
        ? {
            message:
              "小数点の移動が完了！ 商を入力しましょう。",
            kind: "ok"
          }
        : {
            message:
              "筆算の中で、両方の小数点が動きました。",
            kind: "ok"
          };

    render();
  }

  function moveAnswerPoint(direction) {
    if (
      finished ||
      timeIsUp()
    ) {
      return;
    }

    const q =
      questions[selected];

    if (
      !q ||
      operation !==
        "multiply" ||
      q.tasks[
        q.progress
      ]?.type !==
        "answerShift"
    ) {
      return;
    }

    const next =
      q.answerShift +
      direction;

    if (
      next < 0 ||
      next > 4
    ) {
      return;
    }

    q.answerShift = next;

    feedback = {
      message: "",
      kind: ""
    };

    render();
  }

  function completeQuestion(q) {
    q.done = true;
    q.progress =
      q.tasks.length;

    q.input = "";
    score++;

    const next =
      questions.findIndex(
        item => !item.done
      );

    if (next !== -1) {
      selected = next;

      feedback = {
        message:
          "1問正解！ 次の問題です。",
        kind: "ok"
      };

    } else if (
      mode === "timed" &&
      !timeIsUp()
    ) {
      createSheet();

      feedback = {
        message:
          "6問クリア！ 次の問題に進みます。",
        kind: "ok"
      };

    } else {
      feedback = {
        message:
          "全問正解です！",
        kind: "ok"
      };
    }

    render();
  }

  function confirmMultiplicationAnswer() {
    if (
      finished ||
      timeIsUp()
    ) {
      return;
    }

    const q =
      questions[selected];

    if (
      !q ||
      q.done ||
      operation !==
        "multiply" ||
      q.tasks[
        q.progress
      ]?.type !==
        "answerShift"
    ) {
      return;
    }

    if (
      q.answerShift !==
      q.answerPlaces
    ) {
      feedback = {
        message:
          "小数点の位置をもう一度考えましょう。" +
          "元の2つの数の小数部分が、合わせて何桁か数えてみましょう。",
        kind: "ng"
      };

      render();
      return;
    }

    completeQuestion(q);
  }

  function enterKey(key) {
    if (
      finished ||
      timeIsUp()
    ) {
      return;
    }

    const q =
      questions[selected];

    if (
      !q ||
      q.done
    ) {
      return;
    }

    if (
      operation === "divide" &&
      q.divisionShift <
        q.requiredShifts
    ) {
      return;
    }

    const task =
      q.tasks[q.progress];

    if (
      !task ||
      task.type ===
        "answerShift"
    ) {
      return;
    }

    let maxLength;

    switch (task.type) {
      case "answer":
        maxLength = 8;
        break;

      case "quotient":
        maxLength = 1;
        break;

      case "sum":
        maxLength = 5;
        break;

      case "partial":
        maxLength = 3;
        break;

      default:
        maxLength = 3;
    }

    if (key === "⌫") {
      q.input =
        q.input.slice(
          0,
          -1
        );

    } else if (
      key === "."
    ) {
      if (
        task.type ===
          "answer" &&
        !q.input.includes(
          "."
        )
      ) {
        q.input +=
          q.input === ""
            ? "0."
            : ".";
      }

    } else if (
      /^[0-9]$/.test(key) &&
      q.input.length <
        maxLength
    ) {
      q.input += key;
    }

    feedback = {
      message: "",
      kind: ""
    };

    render();
  }

  function checkCurrent() {
    if (
      finished ||
      timeIsUp()
    ) {
      return;
    }

    const q =
      questions[selected];

    if (
      !q ||
      q.done
    ) {
      return;
    }

    if (
      operation === "divide" &&
      q.divisionShift <
        q.requiredShifts
    ) {
      return;
    }

    const task =
      q.tasks[q.progress];

    if (
      !task ||
      task.type ===
        "answerShift"
    ) {
      return;
    }

    if (
      q.input === "" ||
      q.input === "."
    ) {
      feedback = {
        message:
          "黄色い場所に数字を入力してください。",
        kind: "ng"
      };

      render();
      return;
    }

    if (
      Number(q.input) !==
      Number(task.expected)
    ) {
      feedback = {
        message:
          "まだ違います。この段をもう一度考えましょう。",
        kind: "ng"
      };

      render();
      return;
    }

    const completedTask =
      task;

    q.progress++;
    q.input = "";

    if (
      q.progress ===
      q.tasks.length
    ) {
      completeQuestion(q);
      return;
    }

    if (
      operation ===
        "multiply" &&
      q.tasks[
        q.progress
      ].type ===
        "answerShift"
    ) {
      feedback = {
        message:
          "整数の計算は正解！ 最後に答えの小数点を動かしましょう。",
        kind: "ok"
      };

    } else if (
      operation ===
        "divide" &&
      completedTask.type ===
        "quotient" &&
      completedTask.expected ===
        "0"
    ) {
      feedback = {
        message:
          completedTask.step === 0
            ? "正解！ 商に小数点が付きました。引き算を飛ばして0を下ろします。"
            : "正解！ この位は引き算を飛ばして次の商へ進みます。",
        kind: "ok"
      };

    } else {
      feedback = {
        message:
          "正解！ 次の段へ進みます。",
        kind: "ok"
      };
    }

    render();
  }

  /*
    キーボード・物理テンキーにも対応。
    わり算の小数点移動中：左右矢印。
    かけ算の答えの小数点移動中：左右矢印。
  */
  window.addEventListener(
    "keydown",
    event => {
      if (
        $("app").hidden ||
        finished ||
        event.altKey ||
        event.ctrlKey ||
        event.metaKey
      ) {
        return;
      }

      if (timeIsUp()) {
        return;
      }

      const q =
        questions[selected];

      if (
        !q ||
        q.done
      ) {
        return;
      }

      if (
        operation ===
          "divide" &&
        q.divisionShift <
          q.requiredShifts
      ) {
        if (
          event.key ===
          "ArrowRight"
        ) {
          event.preventDefault();
          moveDivision(1);

        } else if (
          event.key ===
          "ArrowLeft"
        ) {
          event.preventDefault();
          moveDivision(-1);
        }

        return;
      }

      if (
        operation ===
          "multiply" &&
        q.tasks[
          q.progress
        ]?.type ===
          "answerShift"
      ) {
        if (
          event.key ===
          "ArrowLeft"
        ) {
          event.preventDefault();
          moveAnswerPoint(1);

        } else if (
          event.key ===
          "ArrowRight"
        ) {
          event.preventDefault();
          moveAnswerPoint(-1);

        } else if (
          event.key ===
          "Enter"
        ) {
          event.preventDefault();
          confirmMultiplicationAnswer();
        }

        return;
      }

      if (
        /^[0-9]$/.test(
          event.key
        )
      ) {
        event.preventDefault();
        enterKey(event.key);

      } else if (
        event.key === "." ||
        event.code ===
          "NumpadDecimal"
      ) {
        event.preventDefault();
        enterKey(".");

      } else if (
        event.key ===
        "Backspace"
      ) {
        event.preventDefault();
        enterKey("⌫");

      } else if (
        event.key ===
        "Enter"
      ) {
        event.preventDefault();
        checkCurrent();
      }
    }
  );

  function choose(newOperation) {
    operation = newOperation;

    $("multiplyChoice").classList.toggle(
      "primary",
      operation === "multiply"
    );

    $("divideChoice").classList.toggle(
      "primary",
      operation === "divide"
    );
  }

  $("multiplyChoice").addEventListener(
    "click",
    () => choose("multiply")
  );

  $("divideChoice").addEventListener(
    "click",
    () => choose("divide")
  );

  $("practiceStart").addEventListener(
    "click",
    () => start("practice")
  );

  $("timedStart").addEventListener(
    "click",
    () => start("timed")
  );

  $("newQuestions").addEventListener(
    "click",
    () => {
      score = 0;
      createSheet();
    }
  );

  $("back").addEventListener(
    "click",
    () => {
      clearInterval(timerHandle);
      $("app").hidden = true;
      $("menu").hidden = false;
    }
  );
})();
</script>
</body>
</html>
