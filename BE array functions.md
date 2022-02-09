	/*
		json null 與 "" 是不同的, 在 FileMaker 沒有完全相等於 null 的常數或函數
			jsonListValues 會把 null 忽略, 但是 "" 不會被忽略
		FileMaker 沒有 null  
		FileMaker 在 list 時會把 "" 忽略
		這個 demo 會介紹 BaseElements plugin 的 array 函數
		BE_ArraySetFromValueList ( valueList {; retainEmptyBoolean } ) 
			把值清單轉成 array, 取得 array 編號
			retainEmptyBoolean 如果不寫或設為 false, 會把 "" 忽略
			如果要保留 "", retainEmptyBoolean 須設為 true
			計算所取得的 array number 可以在計算式中應用,
			也可以存為 local 或 global variable 被調用
			每執行一次就會有一個新的 Array number
			如果重新取值, 應先刪除(BE_ArrayDelete)再執行BE_ArraySetFromValueList
		BE_ArrayDelete ( array )
			刪除指定編號的 array
			對於已經不在使用的 array 應該刪除, 減少記憶空間耗用
		BE_ArrayGetSize ( array )
		BE_ArrayFind ( array ; value )
			取得符合指定值(value, 不區分大小寫)的值順序(valueNumber, 從 1 開始計數)
		BE_ArrayGetValue ( array ; valueNumber )
			取得指定值順序(valueNumber, 從 1 開始計數)的值(value)
		BE_ArrayChangeValue ( array ; valueNumber ; newValue )
			以新值( newValue )取代指定值順序(valueNumber)的值
		update: 
			20220209
	*/
	Let ( 
		[
			// 正常 array
			~array = "[1, 2, 3, 4, 5, null, \"abc\", \"DEF\", \"Ghi\", \"\", \"XX\" ]" ;
			~vlA = 1 & ¶ &  2 & ¶ & 3 & ¶ & 4 & ¶ & 5 & ¶ & "null" & ¶ & "abc" & ¶ & "DEF" & ¶ & "Ghi" & ¶ & "" & ¶ & "XX" ;
			~vlB = List(1; 2; 3; 4; 5; "null"; "abc"; "DEF"; "Ghi"; ""; "XX" );
			// 由 be plugin 從 ~vl 產生 array
			~arrayNA_0 = BE_ArraySetFromValueList ( ~vlA );
			~arrayNA_1 = BE_ArraySetFromValueList ( ~vlA ; True );
			~arrayNB = BE_ArraySetFromValueList ( ~vlB ; True )
		];
		"valueCount ( jsonListKeys ( ~array ; \".\" ): array 的 key 數, 取得 11"
		& ¶ & "  " & ValueCount ( JSONListKeys ( ~array ; "." )) & ¶ &
		"valueCount ( jsonLisValues ( ~array ; \".\" ): array 的 value 數, 取得 10"
		& ¶ & "  " & ValueCount ( JSONListValues ( ~array ; "." )) & ¶ &
		"上述兩者相減可以得到 array 中元素的值為 null 的數量" & ¶ & ¶ &

		"valueCount ( ~vlA ): 用字串以 return 串接的寫法, 取值數量, 會把 \"\" 列入計算, 取得 11"
		& ¶ & "  " & ValueCount ( ~vlA ) & ¶ &
		"valueCount ( ~vlB ): 用 list 的寫法,取值數量, 不會把 \"\" 列入計算, 取得 10"
		& ¶ & "  " & ValueCount ( ~vlB ) & ¶ &
		"用 list 取 portal 的值清單, 這點要注意" & ¶ &
		"取 portal 的值清單時如果要保留空值, 可考慮 executeSQL 或 getNthRecord " & ¶ & ¶ &

		"BE_ArrayGetSize ( ~arrayNA_0 ): 空值 \"\" 被刪除, 取得 10"
		& ¶ & "  " & BE_ArrayGetSize ( ~arrayNA_0 ) & ¶ &
		"BE_ArrayGetSize ( ~arrayNA_1 ): 空值 \"\" 保留, 取得 11"
		& ¶ & "  " & BE_ArrayGetSize ( ~arrayNA_1 ) & ¶ &

		"BE_ArrayGetSize ( ~arrayNB ): 空值 \"\" 不被列入計算, 因為已經被 list 刪掉, 取得 10"
		& ¶ & "  " & BE_ArrayGetSize ( ~arrayNB ) & ¶ &

		"BE_ArrayGetValue ( ~arrayNB ; 3 ): (jsonGetElement 是 zeroBase) BE 的 array 函數是 oneBase, 取得 3" 
		& ¶ & "  " & BE_ArrayGetValue ( ~arrayNB ; 3 ) & ¶ &

		"BE_ArrayFind ( ~arrayNB ; \"Abc\" ): 不區分大小寫, 取得 abc 的順序, 取得 7"
		& ¶ & "  " & BE_ArrayFind ( ~arrayNB ; "Abc" ) & ¶ &

		"BE_ArrayChangeValue ( ~arrayNB ; 3; \"順序三\" ): 給予 value 順序 3 新值: \"順序三\""
		& ¶ & "  " & 
		let ( 
			~change = BE_ArrayChangeValue ( ~arrayNB ; 3 ; "順序三" ) ;
			BE_ArrayGetValue ( ~arrayNB ; 3 ) 
		) 
		& ¶ &

		"BE_ArrayDelete ( ~arrayNB ) 刪除編號為 ~arrayNB 的整個 array 後, 不能再從這個 array 取值"
		& ¶ & "  " & "BE_ArrayGetValue ( ~arrayNB ; 3 ) 取得空值 \"\" "
		& ¶ & "  " & BE_ArrayDelete ( ~arrayNB ) &
		BE_ArrayGetValue ( ~arrayNB ; 3 )
	)