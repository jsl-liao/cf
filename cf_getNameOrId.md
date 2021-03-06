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
	Let (
		[
			/* trial data
			_tfsvl = "l" ;
			_nameOrId = 1 ;
			_layoutName = "" ;
			_ifGetName = true ;
			*/
			// retrive name list
			~names = 
				Case ( 
					Left ( _tfsvl ; 1 ) = "t" or _tfsvl = 1 ; TableNames ( "" ) ;
					Left ( _tfsvl ; 1 ) = "f" or _tfsvl = 2 ; 
						If (
							_layoutName = "" ;
							FieldNames ( "" ; Get ( LayoutName ));
							FieldNames ( "" ; _layoutName )
						);
					Left ( _tfsvl ; 1 ) = "s" or _tfsvl = 3 ; ScriptNames ( "" );
					Left ( _tfsvl ; 1 ) = "v" or _tfsvl = 4 ; ValueListNames ( "" );
					Left ( _tfsvl ; 1 ) = "l" or _tfsvl = 5 ; LayoutNames ( "" )
				);
			// retrive id list
			~ids = 
				Case ( 
					Left ( _tfsvl ; 1 ) = "t" or _tfsvl = 1 ; TableIDs ( "" );
					Left ( _tfsvl ; 1 ) = "f" or _tfsvl = 2 ; 
						If (
							_layoutName = "" ;
							FieldIDs ( "" ; Get ( LayoutName ));
							FieldIDs ( "" ; _layoutName )
						);
					Left ( _tfsvl ; 1 ) = "s" or _tfsvl = 3 ; ScriptIDs ( "" );
					Left ( _tfsvl ; 1 ) = "v" or _tfsvl = 4 ; ValueListIDs ( "" );
					Left ( _tfsvl ; 1 ) = "l" or _tfsvl = 5 ; LayoutIDs ( "" )
				);
			~json =
				If (
					_ifGetName = True ;
					While (
						[
							~i = 1 ;
							~end = ValueCount ( ~ids );
							~j = "{}"
						];
						~i <= ~end ;
						[
							~key = GetValue ( ~ids ; ~i );
							~value = GetValue ( ~names ; ~i );
							~j = JSONSetElement ( ~j ; ~key ; ~value ; 1 ); // jsonString
							~i = ~i + 1
						];
						~j
					);
					While (
						[
							~i = 1 ;
							~end = ValueCount ( ~names );
							~j = "{}"
						];
						~i <= ~end ;
						[
							~key = GetValue ( ~names ; ~i );
							~value = GetValue ( ~ids ; ~i );
							~j = JSONSetElement ( ~j ; ~key ; ~value ; 1 ); // jsonString
							~i = ~i + 1
						];
						~j
					)
				)
		];
		JSONGetElement ( ~json ; _nameOrId )
	)
