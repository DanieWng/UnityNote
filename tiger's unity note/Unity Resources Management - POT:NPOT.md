#Unity Resources Management - POT/NPOT

</br>

## POT:
*	pot는 이미지 사이즈 길이 가로랑 세로 2의 n제곱이라 뜻이다. 이 사아즈를 맞추면 pvrtc 혹은  etc등 이런 gpu texture format로 압축할 수 있는 장점있다. 

*	2d 개발과정에서 pvrtc format는 4444, 2222만 지원하니까 픽셀 깨지는 이슈가 자주 생겼다.

*	개발과정에서 iOS랑 android 따로 리소스 관리하면 문제 더 복잡해지기 때문에 어떻게 하면 좋을까?
	
	1.	이미지 사이즈가 가능하면 256x256, 512x512 넘지 않게
	
	2.	넘은 경우에 
	
		* 더 작게 분리
		* 한 장면에서 이런 사이즈인 이미지 및장 있는지 확인
		* split alpha & pvrtc 
	
	3.	나머지 작은 리소스들 많고 rgba32만 적용해야 할 경우에 pot png로 담고 따로 저장한다. unity - texture import setting - sprite mode:
		
		*	single
		* 	multiple
		*	polygon

		한장으로 담는 이유는 draw call 횟수를 줄릴 수 있으니까요. 
		
		> tutorial: <https://www.youtube.com/watch?v=gbgIA3pwpHc>		