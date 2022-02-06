	/*
		purpose: 
			從 value list 中取出包含某個字串的 values
		format:
			cf_extractValuesFromList ( _valueList; _compareText; _ifStack; _ifCaseSensitive; _ifMatch; _ifLeading; _ifTrailing )
		parameters:
			_valueList: 
				用來取值的 value list
			_compareText: 
				比對的文字
			_ifStack: 
				對找到的值是留下(true)組合成新的值清單, 或者從原清單中刪除(false), not false then true
			_ifCaseSensitive: 
				是否區分比對文字的大小寫, not true then false
			_ifMatch: 
				值與比對的文字要完全相等, not true then false, 是否還要分大小寫, 視 _ifCaseSensitive 而定
			_ifLeading: 
				是否只認定開頭的文字, not true then false
				_ifMatch =  true 時, _ifLeading ㄧ定是 true
				_ifMatch = false 時, _ifLeading 選項才有效
			_ifTrailing: 
				是否只認定結尾的文字,  not true then false; 只有
				_ifMatch =  true 時, _ifTrailing ㄧ定是 true
				在ifMatch = false, _ifTrailing 選項才有效
		dependencies:
		recursive:
			while
		notes:
			_compareText 不可以有 value 分隔符號
			char( 9 ), char( 10 ), char( 13 ), char( 8232 ) 會被 filemaker 視為 value 分隔符號
		version: v3
		by:Jeff Liao
		history:
			initial 20220205 formal named: cf_valuesFromListByPattern
			v2
			v3 20220207, 
				1. 比對文字是否須與找的值是否要完全符合 
				2. 加上可以選擇對找到找的值是保留或者去除
	*/

	case ( 
		_valueList = "" ; "" ;
		_compareText = "" ; "" ;
		patternCount ( _compareText ; char ( 9 )); "" ;
		patternCount ( _compareText ; char ( 10 )); "" ;
		patternCount ( _compareText ; char ( 13 )); "" ;
		patternCount ( _compareText ; char ( 8232 )); "" ;
		while(
			[
				~ifS = if ( _ifStack = false ; 0 ; 1 );
				~ifC = if ( _ifCaseSensitive = true ; 1 ; 0 );
				~ifM = if ( _ifMatch = true ; 1 ; 0 );
				~ifL = case ( ~ifM ; 1 ; _ifLeading = true ; 1 ; 0 );
				~ifT = case ( ~ifM ; 1 ; _ifTrailing = true ; 1 ; 0 );
				~cs = if ( ~ifC ; "«" & char(41162) & "»" ; _compareText );
				~lcs = length ( ~cs );
				~vl = if ( ~ifC ; substitute ( _valueList ; _compareText ; ~cs ) ; _valueList );
				~end = valueCount ( _valueList );
				~i = 1 ;
				~result = ""
			];
			~i <= ~end ;
			[
				~v = getValue ( ~vl ; ~i );
				~v = 
					if ( ~ifS ;
						/* 符合條件的值取出來, 組合成新的值清單 */
						case (
							~ifM and ~v = ~cs ; ~v ;
							~ifM and ~v <> ~cs ; "" ;
							~ifT and right ( ~v ; ~lcs ) = ~cs ; ~v ;
							~ifL and left ( ~v ; ~lcs ) = ~cs ; ~v ;
							not ( ~ifL or ~ifT ) and patternCount ( ~v ; ~cs ) > 0 ; ~v ; 
							"" 
						);
						/* 符合條件的值, 從原值清單中刪除 */
						case (
							~ifM and ~v = ~cs ; "" ;
							~ifM and ~v <> ~cs ; ~v ;
							~ifT and right ( ~v ; ~lcs ) = ~cs ; "" ;
							~ifL and left ( ~v ; ~lcs ) = ~cs ; ~v ;
							not ( ~ifL or ~ifT ) and patternCount ( ~v ; ~cs ) > 0 ; "" ; 
							~v 
						)
					);
				~result = ~result & if ( not isEmpty ( ~v ) ; ~v & ¶ ; "" );
				~i = ~i + 1
			];
			if ( ~ifC ; substitute ( ~result ; ~cs ; _compareText ); ~result )
		)
	)


