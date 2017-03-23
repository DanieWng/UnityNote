#Unity Resources Management - Alpha8
</br>

##Supprot GPU Texture Format

	Format | Description | Support Paltform
	------ | --------- 	 | ---------
	RGBA   |				 |
	RGB    | 				 | 
	PVRTC  | 				 |
	ETC1   | 				 |
	ETC2   | 				 |
	Alpha8 | R,G,B 채널을 제거한다는 뜻이다. | Any OS


##Example:

1.	**Alpha8**
	
	_600x600인 이미지는 rgba32bits로 적용하면 1.4mb 메모리를 쓰겠음._
	
	![](https://github.com/DanieWng/UnityNote/blob/master/sceneshot/alpha8_demo/Jietu20170323-102446.jpg?raw=true)
	
	>실행 테스트:
	
	![](https://github.com/DanieWng/UnityNote/blob/master/sceneshot/alpha8_demo/Jietu20170323-102538.jpg?raw=true)
	
	_alpha8로 적용하면 351.6kb로 줄렸다._
	
	![](https://github.com/DanieWng/UnityNote/blob/master/sceneshot/alpha8_demo/Jietu20170323-102650.jpg?raw=true)
	
	>실행 테스트:
	
	![](https://github.com/DanieWng/UnityNote/blob/master/sceneshot/alpha8_demo/Jietu20170323-102704.jpg?raw=true)
	
	>횐색되니까 더 휴율적으로 활용할 수 있다.
	
	![](https://github.com/DanieWng/UnityNote/blob/master/sceneshot/alpha8_demo/Mar-23-2017%2010-29-00.gif?raw=true)
	
	>원보 파일: <https://github.com/DanieWng/UnityNote/blob/master/sceneshot/alpha8_demo/gradient.png>
	
</br>
</br>
	
##결론:

만약에 이미지가 단색으로 경우에 알파값이랑 같이 저장해서 unity에서 alpha8 format로 적용하는 방법을 추천함.
