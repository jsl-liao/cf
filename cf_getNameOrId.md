<<<<<<< HEAD
=======
# fmp-use-experience
a Chinese using FileMaker Pro
>>>>>>> 8d743271117107c28fcb61f2385f2c0c510bc62a
/*
	purpose: 
		在目前檔案從 name 取得 id, 或者從 id 取得 name; 獲取全部的 id 和 name
	format:
		cf_getNameOrId ( _tfsvl ; _nameOrId ; _layoutName ; _ifGetName )
	parameters:
		_tfsvl: if first character
			t: table
			f: field
			s: script
			v: value list
			l: layout
			如果為了語意上更清楚, 也可以輸入 table, field, script value list...; 只要第一個字元符合即可
		_nameOrId: 輸入 name 或者 id
		_layoutName:
			只有在要取得 field name 和 id 會用到
			如果是空值, 則設定為 current layout; 如果有指定則依據指定的 layout
		_ifGetName:
			valueList 和 script 允許用純數字當名稱, 要避免這個狀況發生的錯誤, 所以使用這個參數
			如果是 true 則是要從 id 取得 name, 否則從 name 取得 id	
	dependencies:
	recursive: while
	notes:
		如果 _nameOrId, _layoutName 都輸入 "" 則會顯示 name:id 樣式的完整 json object
		如果 _nameOrId, _layoutName 都輸入 "" , _ifGetName 輸入 true 或 1 則會顯示 id:name 樣式的完整 json object
	sample id: 
	by:Jeff Liao
	update: 20211226
*/
/* sample
input: 
  cf_getNameOrId ( "l" ; 1 ; "" ; true ) // 查詢 layout id = 1 的 layout name
output: 
  ele
input: 
  cf_getNameOrId ( "l" ; "ele" ; "" ; false ) // 查詢 layout name = ele 的 layout id
output: 
  1
input: 
  cf_getNameOrId ( "l" ; "" ; "" ; true ) // 列出所有 layout id : layout name
output: 
  {"1":"ele","11":"EA","12":"Manage","13":"vl","14":"Trial","15":"LearnPage","18":"CardWindow_EA","19":"Property","26":"CssProperty","27":"CssSelector","3":"Page","30":"ReferencePage","36":"CssFunction","38":"dev","6":"Attribute"}
input: 
  cf_getNameOrId ( "l" ; "" ; "" ; false ) // 列出所有 layout name : layout id
output:
  {"Attribute":"6","CardWindow_EA":"18","CssFunction":"36","CssProperty":"26","CssSelector":"27","EA":"11","LearnPage":"15","Manage":"12","Page":"3","Property":"19","ReferencePage":"30","Trial":"14","dev":"38","ele":"1","vl":"13"}
*/
=======
  cf_getNameOrId ( "l" ; 1 ; "" ; true )
output: 
  ele
input: 
  cf_getNameOrId ( "l" ; "ele" ; "" ; false )
output: 
  1
input: 
  cf_getNameOrId ( "l" ; "" ; "" ; true )
output: 
  {"1":"ele","11":"EA","12":"Manage","13":"vl","14":"Trial","15":"LearnPage","18":"CardWindow_EA","19":"Property","26":"CssProperty","27":"CssSelector","3":"Page","30":"ReferencePage","36":"CssFunction","38":"dev","6":"Attribute"}
input: 
  cf_getNameOrId ( "l" ; "" ; "" ; false )
output:
  {"Attribute":"6","CardWindow_EA":"18","CssFunction":"36","CssProperty":"26","CssSelector":"27","EA":"11","LearnPage":"15","Manage":"12","Page":"3","Property":"19","ReferencePage":"30","Trial":"14","dev":"38","ele":"1","vl":"13"}
*/
>>>>>>> 8d743271117107c28fcb61f2385f2c0c510bc62a
