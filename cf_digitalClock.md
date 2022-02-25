  /*
    purpose:
      產生 web viewer 的數字時鐘
    format:
      cf_digitalClock ( _backgroundColor; _fontFamily; _fontSize; _color; _letterSpacing )
    parameters:
      _backgroundColor: 背景色
      _fontFamily: 字型
      _fontSize: 字體大小, 包含單位
      _color:
      _letterSpacing:字元間距, 包含單位
      參數為 css 屬性, 要依照 css 文法撰寫
    dependencies:
    returns:
      應用 web viewer 的 html
    recursive:
    notes:
      
    current version: v1
    by jeff liao
    initial: 20220225
    history:
      v1 always center

      
  */

  let (
    [
      /* sample parameters begin
      _backgroundColor = "#800" ;
      _fontFamily = "helvetica" ;
      _fontSize = "1.5rem" ;
      _color = "cyan" ;
      _letterSpacing = "6px" ;
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
            <meta version='v1'>
            <title>digital clock</title>
            <style>
              :root{
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
                top: 50%;
                left: 50%;
                transform: translateX(-50%) translateY(-50%);
                color: var(--color);
                font-size: var(--font-size);
                font-family: var(--font-family);
                letter-spacing: var(--letter-spacing);
              }
            </style>
          </head>

          <body>
            <div id='myClock' class='clock' onload='showTime()'></div>
            <script>
              function showTime() {
                let date = new Date();
                let hour = date.getHours();
                let minute = date.getMinutes();
                let second = date.getSeconds();
                let am_pm = 'AM';
                if ( hour == 0 ) {
                  hour = 12;
                }
                if ( hour > 12 ) {
                  hour = hour - 12;
                  am_pm = 'PM';
                }
                hour = ( hour < 10 ) ? '0' + hour : hour ;
                minute = ( minute < 10 ) ? '0' + minute : minute;
                second = ( second < 10 ) ? '0' + second : second ;
                let time = hour + ':' + minute + ':' + second + ' ' + am_pm ;
                document.getElementById('myClock').innerText = time ;
                setTimeout(showTime, 1000);
              };
              showTime();
            </script>
          </body>
        </html>
        ";
      ~backgroundColor = if ( _backgroundColor = "" ; "--background-color: black;" ; "--background-color: " & _backgroundColor & ";" );
      ~fontFamily = if ( _fontFamily = "" ; "--font-family: monaco mono;" ; "--font-family: " & _fontFamily & ";" );
      ~fontSize = if ( _fontSize = "" ; "--font-size: 14px;" ; "--font-size: " & trimAll ( _fontSize ; 1 ; 3 ) & ";" );
      ~color = if ( _color = "" ; "--color: white;" ; "--color: " & _color & ";" );
      ~letterSpacing = if ( _letterSpacing = "" ; "--letter-spacing: 3px;" ; "--letter-spacing: " & trimAll ( _letterSpacing ; 1 ; 3 ) & ";" );
      ~html = substitute ( ~html ;
        [ "--background-color: black;" ; ~backgroundColor ];
        [ "--font-family: monaco mono;" ; ~fontFamily ];
        [ "--font-size: 14px;" ; ~fontSize ];
        [ "--color: white;" ; ~color ];
        [ "--letter-spacing: 3px;" ; ~letterSpacing ]
      )
    ];
    "data:text/html," & ~html
  )
