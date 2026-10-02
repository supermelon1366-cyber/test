
const SHEET_NAME = "data";
const HEADERS = ["작성 시간", "별명", "응원 메시지"];

// 시트와 컬럼 자동 생성 및 보완
function getDataSheet() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  let sheet = ss.getSheetByName(SHEET_NAME);

  if (!sheet) {
    sheet = ss.insertSheet(SHEET_NAME);
  }

  // 첫 번째 행의 기존 헤더 확인
  const lastCol = Math.max(sheet.getLastColumn(), 1);
  const currentHeaders = sheet.getRange(1, 1, 1, lastCol)
    .getValues()[0];

  // 필요한 헤더가 없으면 마지막 컬럼 뒤에 추가
  HEADERS.forEach(header => {
    if (!currentHeaders.includes(header)) {
      const newCol = sheet.getLastColumn() + 1;
      sheet.getRange(1, newCol).setValue(header);
      currentHeaders.push(header);
    }
  });

  // 헤더 서식 설정
  sheet.getRange(1, 1, 1, sheet.getLastColumn())
    .setFontWeight("bold")
    .setBackground("#d9ead3");

  sheet.setFrozenRows(1);

  return sheet;
}

// 웹사이트에서 데이터를 받는 함수
function doPost(e) {
  try {
    const params = e.parameter || {};

    // JSON 또는 폼 데이터 처리
    let data = params;
    if (e.postData && e.postData.type &&
        e.postData.type.includes("application/json")) {
      data = JSON.parse(e.postData.contents);
    }

    const nickname = String(data.nickname || "").trim();
    const message = String(data.message || "").trim();

    // 입력값 검사
    if (!nickname || !message) {
      return jsonResponse({
        success: false,
        message: "별명과 응원 메시지를 입력해 주세요."
      });
    }

    if (nickname.length > 30 || message.length > 500) {
      return jsonResponse({
        success: false,
        message: "별명은 30자, 메시지는 500자 이내로 입력해 주세요."
      });
    }

    const lock = LockService.getScriptLock();
    lock.waitLock(10000);

    try {
      const sheet = getDataSheet();

      // 헤더 이름에 맞게 저장 위치 확인
      const headers = sheet.getRange(
        1, 1, 1, sheet.getLastColumn()
      ).getValues()[0];

      const row = new Array(headers.length).fill("");
      row[headers.indexOf("작성 시간")] = new Date();
      row[headers.indexOf("별명")] = nickname;
      row[headers.indexOf("응원 메시지")] = message;

      sheet.appendRow(row);
    } finally {
      lock.releaseLock();
    }

    return jsonResponse({
      success: true,
      message: "응원 메시지가 등록되었습니다."
    });

  } catch (error) {
    return jsonResponse({
      success: false,
      message: "오류가 발생했습니다."
    });
  }
}

// JSON 응답 생성
function jsonResponse(data) {
  return ContentService
    .createTextOutput(JSON.stringify(data))
    .setMimeType(ContentService.MimeType.JSON);
}

// 최초 실행 시 시트 준비
function setup() {
  getDataSheet();
}
