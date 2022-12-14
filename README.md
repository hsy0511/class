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
### 회원번호의 자동완성은 code라는 매개변수의 밸류 값을 회원명을 선택할 때 마다 그 값에 해당하는 회원번호로 지정해주면서 자동완성을 시켜줍니다.

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

# 수강신청_p

```
request.setCharacterEncoding("UTF-8");
String sql = "insert into tbl_class_202201 values (?,?,?,?,?)";

Connection conn = DBConnect.getConnection();
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, request.getParameter("resist_month"));
pstmt.setString(2, request.getParameter("c_no"));
pstmt.setString(3, request.getParameter("class_area"));
pstmt.setInt(4, Integer.parseInt(request.getParameter("tuition")));
pstmt.setString(5, request.getParameter("class_name"));

pstmt.executeUpdate();
```

### 수강신청_p.jsp와 bd를 연동시켜서 5개에 값을 꽂아넣으면 멤버 테이블에 추가가 될수있게 만든 코드입니다. tuition는 형변환를 시켜서 integer형으로 받아주고 다른 4개는 get.parameter를 통해 사용자가 적은 값을 string형으로 세팅해주고 pstmt.executeUpdate를 통해서 테이블에 추가가 됩니다. 추가가 끝나면 자동으로 인덱스 창으로 넘어갑니다.

# 회원정보조회
![1](https://user-images.githubusercontent.com/104752580/207786913-ad13f35f-63ca-407a-8761-0dba273cb584.JPG)

## 쿼리문
```
String sql = "select substr(c.resist_month,1,4)||'년'||substr(c.resist_month,5,2)||'월' resist_month,
c.C_NO, m.C_NAME, t.CLASS_NAME, c.CLASS_AREA,
to_char(CLASS_PRICE,'L999,999')CLASS_PRICE,
m.GRADE from TBL_TEACHER_202201 t, TBL_MEMBER_202201 m, TBL_CLASS_202201 c
where c.C_NO=m.C_NO and c.TEACHER_CODE = t.TEACHER_CODE";
```
### 수강월을 00년 00월로 나타내기 위해서 substr을 활용하였고, 수강료의 원표시를 표기하기 위해서 to_char를 활용하였다. 3개의 테이블을 조인시키며 쿼리문을 완성시켰다.

## 테이블
```
<table class= "table_line">
<tr>
<th>수강월</th><th>회원번호</th><th>회원명</th><th>강의명</th><th>강의장소</th><th>수강료</th><th>등급</th>
</tr>
<% while(rs.next()){ %>
<tr>
<td><%= rs.getString(1) %></td>
<td><%= rs.getString(2) %></td>
<td><%= rs.getString(3) %></td>
<td><%= rs.getString(4) %></td>
<td><%= rs.getString(5) %></td>
<td><%= rs.getString(6) %></td>
<td><%= rs.getString(7) %></td>
</tr>
<% } %>
</table>
```
### rs.getString을 while문 안에서 반복시키며 값을 출력했습니다. 

# 강사매출현황
![1](https://user-images.githubusercontent.com/104752580/208007941-6a8114ad-5267-4ae2-b053-b7c7ea265e4c.JPG)
## 쿼리문
```
String sql = "select t.teacher_code, t.teacher_name, t.class_name, 
to_char(sum(t.CLASS_PRICE),'L999,999') total 
from TBL_TEACHER_202201 t, TBL_CLASS_202201 c 
where t.teacher_code = c.teacher_code
group by t.teacher_code, t.teacher_name, t.class_name
order by t.teacher_code";

```
### 강사 코드가 겹치는 강사 테이블과 클래스 테이블을 조인하고 강사코드에 따른 수강료를 sum함수로 더하며 총매출 값을 출력합니다.
## 테이블
```
<table class= "table_line">
<tr>
<th>강사코드</th><th>강사명</th><th>강의명</th><th>총매출</th>
</tr>
<% while(rs.next()){ %>
<tr>
<td><%= rs.getString(1) %></td>
<td><%= rs.getString(2) %></td>
<td align="right"><%= rs.getString(3) %></td>
<td align="right"><%= rs.getString(4) %></td>
</tr>
<% } %>
</table>
```
### rs.getString을 while문 안에서 반복시키며 값을 출력했습니다. 
