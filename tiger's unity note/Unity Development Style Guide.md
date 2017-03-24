# Unity Development Style Guide


</br>
</br>

##유니티 리소스 최적화

*	**유니티에 대한 최적화가 크게 3가지로 나눌 수 있음:**
	1.	메모리  
	2. 앱 파일 크기 - (ipa, apk)
	3. 앱 설치 필요한 하드 공간

###메모리 소유 줄리기:
	1. texture
	2. audio
	3. font

_메모리를 가장 많이 쓰는 리소소들임._

###Texture:
* **size**
	*	9 patch png
	* 	pot기준 가능하도록 맞춤
	
* **pot & npot**
pot란 가로랑 세로 길이 2의 제곱인 정사각형 이미지이다. 예 8x8, 16x16, 32x32, 64x64, 128x128. npot는 pot 아닌  뜻이다. pot 권장하는 이유는 PVRTC format 적용할 수 있는 점이다. 다만 유니티는 pvrtc8888 없지만  pvrtc4bits는 화질 손실 현상은 직접 디바이스에서만 체크할 수 있음. pvrtc4bits 적용때 ETC1처럼 알파 채널을 따로 분리시키는 방법을 추천함.

	_iOS에서 정사각형을 요구한다._

* **반복되는 픽셀 처리**
 texture - wrap mode - clamp/repeat
 	* clamp:
 	* repeat:
 	</br> 
	_***Image로 repeat 적용할 경우에 백터랑 삼각형이 많이 생길 텐데 이거 raw image로 해야 함.***_ 

* **Render Format**
	제가 개발때 가장 많이 쓰이는 포멧이 pvrtc, etc1, rgba16, rgba32, alpha8...
	
	Format | Description | Spec
	--------- | --------- | ---------
	pvrtc||모든 iOS, 일부 안드로이드
	etc1|4bits, alpha channel 없음|모든 안드로이드
	etc2|8bits, alpha channel 있음|OpenGL 3.0 지원된 안드로이드, _아니면 RGBA32로 전환_  
	rgba16||
	tgba32||
	alpha8||
 
	1. *rgb without alpha channel*</br>
	이미지 소스가 알파값 없으면 pvrtc, etc1, alpha8로 추천, pvrtc경우에 픽셀 많이 깨질 데가 바로 오퍼시티 적용하는 부분임.
	2. *rgb with split alpha channel*</br>
	참고 자료: &lt;http://developers.mobage.jp/blog/texture-compression
	
	>format에 따라 메모리 비교: &lt;http://unching-star.hatenablog.jp/entry/2013/06/24/095510
	
	####rgb & alpha 분리시킨 pvr4bits 테스트 결과:
	*	RGBA32, RGBA16, PVR4, split alpha channel with pvr4는 iOS에 많이 쓰이는 포멧임.
		![](https://raw.githubusercontent.com/DanieWng/UnityNote/master/sceneshot/Mar-21-2017%2010-27-27.gif)
	
	*	메모리 소요 비교:    
		![](https://github.com/DanieWng/UnityNote/blob/master/sceneshot/Mar-21-2017%2010-24-47.gif?raw=true)
		![](https://github.com/DanieWng/UnityNote/blob/master/sceneshot/Jietu20170321-115602.jpg?raw=true)
	*	리소소 코드: 
	
	* 	적용 방법: 
	
		1.	iOS는 pvrtc4bits, 안드로이드는 etc1로 포멧으로 필수, 또 pvrtc는 사이즈가 2의 n제곱인 적사작형 이어야 하는 조건있음.
		2.	alpha 텍스처랑 rgb텍스처 런더링 셰이더
		3. material 
		4. sprite나 ui/image 따로 적용

	*	png 파일 선처리
	
		1.	npot는 pot로 컨버트 방법 (**난이도 순차**)
			*	[ ] 포토샨에 png export때 미리 pot 사이즈로 뽑기
			*	[ ] 자동 컨버트 툴이나 명령어 개발해서 npot이더라도 더 크게 해당되는 pot사이즈로 전환할 수 있도록 함. 이게도 디자인 작업				때 가능하면 pot사이즈 조금 넘는 일은 피해야 한다. 이미지가 256 이하면 별 상관없는데 512부터 많이 커질 거라서 예를 들어 				400x580인 이미지를 컨버트하면 512를 넘으니까 1024x1024로 해야 한다.
			* 	[ ] 유니티에 spriter packer를 통해 자동 알파 채널을 분리하고 몪어서 큰 atlas를 만들는 방법 찾기. 기준에 안드로이드 				etc1 경우에 이미 자동 되어 있음.  
			
		2. alpha & rgb channel 분리
			*	유니티에서
			* 	명령어 써서 따로 
 
* **gradient, shadow는 가능하면 따로 빼서 유니티에서 처리** 
	*	Gradient:
		*	gradient component 추가, start color& end color로 생선
		*	이미지 사이즈가 크면 축소 시켜서 작은 사이즈로 9patch png처럼 적용해도 됨. 

* **Texture Tools**
	1. ImageMagick
		*	이미지 소스는 rgb & alpha 분리시키는 일:
	
		```python
			// get rgb.png
			convert SOURCES_PATH -alpha Off DES_PATH
			
			// get alpha.png
			convert SOURCES_PATH -channel A -separate DES_PATH
		```
	2. PVRTexTool (GPU Texture Tool)
		

</br>
</br>

###특수 폴더:

* Assets/Resources 
* Assets/StreamingAssets 

예시: 
`이미지 소스 -> 256x256, 20kb 
-> unity 프로젝트로 삽입 texture2d로 변경`

**활용 방식에 따라 필드 후 있는 위치도 달라짐:**
`1. Resources에 넣고 hierachy에서 쓰지 않은 경우 -> resources.assets
2. Resources에 넣고 hierachy에서 쓰는 경우 -> sharedassets0.assets 
3. StreamingAssets에 넣고 그대로(파일 크기 기준) 
4. 두 폴더가 아닌 경우는 쓰이면 쓰는 game object에 따라, 아니면 xcode 프로젝트 안에 들어오지 않은.`

_**Resources**_:
폴더 안에 이미지 소스 저장경우는 전환할때 파일 자체 움직이는 것 아니라 TGA라는 파일로 저장, 파일 크기는 해당 메모리 크기임.
다만 ipa 뽑을때 압축해서 진행하다. 그래서 어떤 앱은 ipa가 50mb인데 설치해서 200mb로 커진 현상 생긴다. 바로 이 문제다.  

단점:
1. 앱 켜볼 때 이 폴더를 한번 로딩할 거라 커질 수록 첫 화면이 나올때까지 오래 기다림.
2. 안에 있는 텍스처 파일은 커짐.

_**StreamingAssets**_:
안에 있는 파일 그대로 압축하지 않고 저장함.
***활용 방법:***
1. 비트 파일(읽기만)
2. 앱 내부에 동적 로딩할 소스들이 많고 사이즈가 클 때, 압축 패키지로 저장하고 첫 실행 때 풀어 extend directory로 복사하여 저장해서 나중에 직접 쓰는 방식.
3. asset bundle (필요할 때만)

단점: 
1. WWW 로딩 방식만 적용 가능, iOS에서 File로 읽어올 수 있지만 android에서 비동기라서 안됨.
2. 파일 보안성, 이 폴더에 있는 소스가 압축하지 않으니까 확인할 수 있음.
3. 삭제 불가능


_**Scene.untiy**_: 
추후 

_**AssetBundle**_ 
온라인 다운로드 하지 않으면 권장 하지 않다.
_asset bundle중에 공통 리소스를 잘 관리하여 따로 패키징 해야함. 아니면 bundle마다 같은 리소스를 패키징하면 엄청 커질 수 있음._

</br>
</br>
##Unity로 개발한 ipa파일에서 리소스를 가져오기
1.	ipa파일 다운받기
2.	뒤 파일 포멧 이름은 zip로 변경
3. 압축 풀기
4. unity studio로 압축한 패키지를 읽기

</br>
</br>
##유니티 코드 최적화

##Code Style


</br>
</br>




