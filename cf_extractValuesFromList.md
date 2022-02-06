	/*
	purpose: 
		從值清單中取出包含某個字串的值, 組成新清單
	format:
		cf_extractValuesFromList ( _valueList; _compareText;_ifCaseSensitive )
	parameters:
		_valueList: 用來取值的值清單
		_compareText: 比對的文字
		_ifCaseSensitive: 是否區分比對文字的大小寫, not true then false
	dependencies:
	recursive:
		while
	return:
		filtered values
	notes:
	version: v1
	by:Jeff Liao
	history:
		initial 20220205
	*/

	case ( 
	/* 
		_valueList = Get(InstalledFMPlugins);
		_compareText = "base";
		_ifCaseSensitive = 0;
	*/
		_valueList = "" ; "" ;
		_compareText = "" ; "" ;
		while(
			[
				~ifC = if ( _ifCaseSensitive = true ; 1 ; 0 );
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
				~v = case ( patternCount ( ~v ; ~cs ) ; ~v ; "" );
				~result = ~result & if ( not isEmpty ( ~v ) ; ~v & ¶ ; "" );
				~i = ~i + 1
			];
			if ( ~ifC ; substitute ( ~result ; ~cs ; _compareText ); ~result )
		)
	)
