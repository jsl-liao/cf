  /*
    purpose:
      產生 web viewer 的數字時鐘
    format:
      cf_digitClock ( _bgColor; _font; _size; _color; _spacing; _isToTop; _vDist; _isToLeft; _hDist; _is0; _ifDate; _ifMonthName; _ifDayName; _ifZh; _is24; _isSec )
    parameters:
      以下參數在 css 中使用
      _bgColor: 背景色
      _font: 字型, font-family
      _size: 字體大小, 包含單位 font-size
      _color: 字元顏色
      _spacing:字元間距, 包含單位 letter-spacing
      _isToTop: 是否向上對齊
      _vDist: 到垂直指定邊的距離
      _isToLeft: 是否向左對齊
      _hDist: 到水平指定邊的距離
      以下參數在 js 中使用
      _is0: 如果數字只有 1 位數, 是否加上前置 0
      _ifDate: 是否顯示日期
      _ifMonthName: 是否用名稱顯示月份, 中文則是 '一','二','三'...; 英文則是 'Jan','Feb','Mar'...
      _ifDayName: 是否顯示星期
      _ifZh: 是否使用國曆顯示日期
      _is24: 是否顯示 24 小時或者 12 小時及 AM/PM
      _isSec: 是否顯示秒數
      _if 開頭的參數為 boolean value, if not true then false
      _is 開頭的參數為 boolean value, if not false then true
      非_is,_if 開頭的參數為 css 屬性, 要依照 css 文法撰寫
    dependencies:
    returns:
      應用 web viewer 的 html
    recursive:
    notes:
      css 單位: https://www.w3schools.com/css/css_units.asp
      css 色彩值: https://www.w3schools.com/cssref/pr_text_color.asp
    current version: v5
    by jeff liao
    initial: 20220225
    history:
  */

  let (
    [
      /* sample parameters begin
      _bgColor = "#800" ;
      _font = "helvetica" ;
      _size = "4rem" ;
      _color = "cyan" ;
      _spacing = "6px" ;
      _isToTop = true ;
      _vDist = "10px" ;
      _isToLeft = false;
      _hDist = "10px";
      _is0 = true ;
      _ifDate = true ;
      _ifMonthName = false ;
      _ifDayName = true ;
      _ifZh = false ;
      _is24 = false ;
      _isSec = true ;
      sample parameters end */
      ~html =
        "
        <!DOCTYPE html>
        <html lang='en'>

          <head>
            <meta charset='UTF-8'>
            <meta http-equiv='X-UA-Compatible' content='IE=edge'>
            <meta name='viewport' content='width=device-width, initial-scale=1.0'>
            <meta author='jeff liao'>
            <meta works on FileMaker web viewer>
            <meta version='v1'>
            <title>digital clock</title>
            <style>
              :root {
                font-size: 16px;
                --background-color: black;
                --font-family: monaco mono;
                --font-size: 14px;
                --color: white;
                --letter-spacing: 3px;
              }

              body {
                background: var(--background-color);
              }

              .clock {
                position: absolute;
                top: 10%;
                left: 10%;
                color: var(--color);
                font-size: var(--font-size);
                font-family: var(--font-family);
                letter-spacing: var(--letter-spacing);
              }
            </style>
          </head>

          <body>
            <div id='myClock' class='clock' onload='showTime()'></div>
            <script type='application/javascript'>
              function showTime() {
                let now = new Date(),
                  year = now.getFullYear(),
                  month = now.getMonth(),
                  dayOfMonth = now.getDate(),
                  dayOfWeek = now.getDay(),
                  hours = now.getHours(),
                  minutes = now.getMinutes(),
                  seconds = now.getSeconds(),
                  am_pm = 'AM',
                  yD = '/',
                  mD = '/',
                  dD = ' ',
                  tD = ':',
                  _is0 = true,
                  _ifDate = true,
                  _ifDayName = true,
                  _ifMonthName = false,
                  _ifZh = true,
                  _is24 = false,
                  _isSec = true,
                  monthNo,
                  dateSentence,
                  timeSentence,
                  interval = (_isSec) ? 1000 : 60000,
                  monthNames = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
                  dayNames = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
                if (_ifZh) {
                  year = year - 1911;
                  dayNames = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
                  monthNames = ['一', '二', '三', '四', '五', '六', '七', '八', '九', '十', '十ㄧ', '十二'];
                  yD = '年';
                  mD = '月';
                  dD = '日';
                }
                let dayName = dayNames[dayOfWeek];
                let monthName = monthNames[month];
                if (_is24 !== true) {
                  if (hours === 0) {
                    hours = 12;
                  } else if (hours > 12) {
                    hours = hours - 12;
                    am_pm = 'PM';
                  }
                } else {
                  am_pm = '';
                };
                if (_is0) {
                  monthNo = ((month + 1) < 10) ? '0' + (month + 1) : month + 1;
                  dayOfMonth = (dayOfMonth < 10) ? '0' + dayOfMonth : dayOfMonth;
                  hours = (hours < 10) ? '0' + hours : hours;
                  minutes = (minutes < 10) ? '0' + minutes : minutes;
                  seconds = (seconds < 10) ? '0' + seconds : seconds;67
                };
                if (_ifDate) {
                  daySentence = year + yD + monthNo + mD + dayOfMonth + dD;
                } else {
                  daySentence = '';
                };
                if (_ifMonthName) {
                  daySentence = year + yD + monthName + mD + dayOfMonth + dD;
                };
                if (_ifDayName) {
                  daySentence = daySentence + ' ' + dayName;
                };
                if (_isSec) {
                  timeSentence = hours + tD + minutes + tD + seconds + ' ' + am_pm;
                } else {
                  timeSentence = hours + tD + minutes + ' ' + am_pm;
                };
                document.getElementById('myClock').innerText = daySentence + ' ' + timeSentence;
                setTimeout(showTime, interval);
              };
              showTime();
            </script>
          </body>

        </html>
        ";
      ~vertical = if ( _isToTop = false ; "bottom: " & trimAll ( _vDist ; 1 ; 3 ) & ";" ; "top: " & trimAll ( _vDist ; 1 ; 3 ) & ";" );
      ~horizontal = if ( _isToLeft = false ; "right: " & trimAll ( _hDist ; 1 ; 3 ) & ";" ; "left: " & trimAll ( _hDist ; 1 ; 3 ) & ";" );
      ~backgroundColor = if ( _bgColor = "" ; "--background-color: black;" ; "--background-color: " & _bgColor & ";" );
      ~fontFamily = if ( _font = "" ; "--font-family: monaco mono;" ; "--font-family: " & _font & ";" );
      ~fontSize = if ( _size = "" ; "--font-size: 14px;" ; "--font-size: " & trimAll ( _size ; 1 ; 3 ) & ";" );
      ~color = if ( _color = "" ; "--color: white;" ; "--color: " & _color & ";" );
      ~letterSpacing = if ( _spacing = "" ; "--letter-spacing: 3px;" ; "--letter-spacing: " & trimAll ( _spacing ; 1 ; 3 ) & ";" );
      ~is0 = if ( _is0 = false ;  "_is0 = false" ; "_is0 = true" );
      ~ifDate = if ( _ifDate = true ; "_ifDate = true" ; "_ifDate = false" );
      ~ifMonthName =
        case (
          not _ifDate ; "_ifMonthName = false" ;
          _ifMonthName ; "_ifMonthName = true" ;
          "_ifMonthName = false"
        );
      ~ifDayName =
        case (
          not _ifDate ; "_ifDayName = false" ;
          _ifDayName ; "_ifDayName = true" ;
          "_ifDayName = false"
        );
      ~ifZh =
        case (
          not _ifDate ; "_ifZh = false" ;
          _ifZh ; "_ifZh = true" ;
          "_ifZh = false"
        );
      ~is24 = if ( _is24 = false ; "_is24 = false" ; "_is24 = true" );
      ~isSec = if ( _isSec = false ; "_isSec = false" ; "_isSec = true" );
      ~html = substitute ( ~html ;
        [ "--background-color: black;" ; ~backgroundColor ];
        [ "--font-family: monaco mono;" ; ~fontFamily ];
        [ "--font-size: 14px;" ; ~fontSize ];
        [ "--color: white;" ; ~color ];
        [ "--letter-spacing: 3px;" ; ~letterSpacing ];
        [ "top: 10%;" ; ~vertical ];
        [ "left: 10%;" ; ~horizontal ];
        [ "_is0 = true" ; ~is0 ];
        [ "_ifDate = true" ; ~ifDate ];
        [ "_ifDayName = true" ; ~ifDayName ];
        [ "_ifMonthName = false" ; ~ifMonthName ];
        [ "_ifZh = true" ; ~ifZh ];
        [ "_is24 = false" ; ~is24 ];
        [ "_isSec = true" ; ~isSec ]
      )
    ];
    "data:text/html," & ~html
  )

