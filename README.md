# main 화면

![image](https://user-images.githubusercontent.com/104752580/207498188-258d2ad8-507a-4858-913f-e7b3780f3d74.png)
수강 신청의 첫 페이지 입니다.

## vDisplay함수
![image](https://user-images.githubusercontent.com/104752580/207498249-fe821f91-006e-44ac-89ad-f33bb1015679.png)
```
   function vDisplay(code) {
      document.classData.c_no.value = code;
      document.classData.class_name.value = "none";
      document.classData.tuition.value = "";
   }
```
### 회원번호의 자동완성을 회원번호가 매개변수로 밸류값으로 전송되고 code로 설정하여 자동완성으로 만들어 줍니다. 수강료 부분도 이와 마찬가지로 강의명의 밸류값을 수강료가 매개변수로 밸류값에 전송되고 tcode로 설정하여 자동완성을 해줍니다.

## 유효성 검사

![image](https://user-images.githubusercontent.com/104752580/207498499-46fab4d0-2bc9-4627-bdef-dcdc4fe77922.png)

```
   function chkValue() {
      var cls = document.classData;
      
      if(!cls.resist_month.value) {
         alert("수강월이 입력되지 않았습니다!");
         cls.resist_month.focus();
         return false;
      }
```

## 번호에 따른 할인율

![image](https://user-images.githubusercontent.com/104752580/207498712-4fa0735c-13b8-4422-8234-7b1107258fdf.png)

```
   function calTuition(tcode) {
      var mbr = document.classData.c_no.value;
      if(!mbr) {
         alert("회원명을 먼저 선택하세요.");
         document.classData.class_name[0].selected = true;
         document.classData.c_name.focus();
      } else {
         
         var salePrice = 0;
         switch (tcode) {
            case "100":
               salePrice = 100000;
               break;
            case "200":
               salePrice = 200000;
               break;
            case "300":
               salePrice = 300000;
               break;
            case "400":
               salePrice = 400000;
               break;
         }
         
         if(mbr.charAt(0)=='2') {
            alert("수강료가 50% 할인 되었습니다.");
            salePrice = salePrice / 2;
         }
```
###  수강료는 강사코드가 100이면 100000원 200이면 200000원 300이면 300000원 400이면 400000원으로 받을 수 있게 case 문을 작성하였습니다. 그리고 회원번호가 20000이상이면 수강료가 50% 할인하는 것을 if 문으로 작성하였습니다.

## 초기화

![image](https://user-images.githubusercontent.com/104752580/207498885-b3533ae8-f315-491c-a38c-812bdc133408.png)

```
<input type="reset" value="다시쓰기" onclick="alert('다시작성합니다')">
```
### 버튼 타입을 리셋으로 설정한 후 온클릭을 설정하여 문구를 띄워주게 작성하였습니다.
