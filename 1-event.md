# 이벤트 처리

## click 이벤트 처리

### 버튼 하나에 클릭 이벤트 핸들러 등록하기
- 요구사항
  - 버튼을 클릭하면 이미지를 화면에서 안보이게 한다.
- 코딩 가이드
  - button과 img 태그에 각각 아이디를 부여한다.
  - 아이디 선택자로 button 엘리먼트를 선택하고 버튼에 클릭 이벤트 핸들러를 등록한다.
  - 이벤트 핸들러 함수에서 아이디 선택자로 img 엘리먼트를 선택하고, .hide()메소드를 실행해서 화면에서 보이지 않게 한다.

```html
<div id="box">
    <img src="photo.png" id="img-photo">
    <button id="btn-hide">감추기</button>
</div>

<script>
    $("#btn-hide").click(function() {
        $("#img-photo").hide();
    });
</script>
```

### 같은 동작을 수행하는 버튼 여러 개에 이벤트 핸들러 등록하기
- 요구사항
  - 경력사항을 등록하는 입력필드가 여러 개 있고, 각 입력필드마다 삭제 버튼이 있다.
  - 삭제 버튼을 클릭하면 해당 경력사항을 삭제한다.
- 코딩 가이드
  - 삭제 버튼을 클릭하면, 삭제버튼이 포함된 div 엘리먼트가 삭제되어야 하기 때문에 div엘리먼트와 button엘리먼트 서로 연관된 값을 가지도록 설정하자.
  - div 엘리먼트에 아이디를 부여하자.
  - button 엘리먼트에 data-xxx 속성으로 div 엘리먼트의 아이디를 설정한다.
  - &lt;div id="box"&gt;엘리먼트의 자손 엘리먼트 중에서 button 엘리먼트를 전부 선택해서 한번에 이벤트 핸들러를 등록한다.
  - 이벤트 핸들러 함수 안에서는 this 키워드로 이벤트가 발생한 button 엘리먼트를 획득하고, 해당 엘리먼트의 data-xxx 속성값을 읽어서 삭제할 div 엘리먼트의 아이디를 알아낸다.

```html
<div id="box">
    <p>경력사항을 등록하세요</p>
    <div id="form-group-1" class="form-group">
        <label>경력사항</label>
        <input type="text" name="career">
        <button data-group="#form-group-1">삭제</button>
    </div>
    <div id="form-group-2" class="form-group">
        <label>경력사항</label>
        <input type="text" name="career">
        <button data-group="#form-group-2">삭제</button>
    </div>
    <div id="form-group-3" class="form-group">
        <label>경력사항</label>
        <input type="text" name="career">
        <button data-group="#form-group-3">삭제</button>
    </div>
</div>

<script>
    $("#box button").click(function() {
        let groupId = $(this).attr("data-group");
        $(groupId).remove();
    });
</script>
```

### 버튼을 클릭했을 때 텍스트 변경하기

- 요구사항
  - 마이너스, 플러스 버튼을 클릭할 때마다 구매가격이 변경되도록 한다.
- 코딩가이드
  - 값을 읽어오고, 값을 변경할 엘리먼트에 고유한 아이디를 부여하자.
  - 가격을 표현하는 엘리먼트에는 data-xxx 속성으로 가격을 관리하고, 화면에 표시되는 값을 ","를 포함해서 표시하자.
  - 버튼을 클릭할 때마다 입력필드의 현재 값을 읽어와서 1증가, 1감소 시키고, refreshOrderPrice() 함수를 실행해서 구매가격을 갱신한다.

```html
<div id="box">
    <table border="1">
        <tr>
            <th>상품명</th>
            <th>가격</th>
            <th>수량</th>
            <th>구매가격</th>
        </tr>
        <tr>
            <td>수채화물감</td>
            <td><strong id="price-txt" data-price="12000">12,000</strong> 원</td>
            <td>
                <button id="btn-minus">-</button>
                <input type="text" name="qty" id="qty" name="qty" value="2" size="5"/>
                <button id="btn-plus">+</button>
            </td>
            <td><strong id="order-price-txt" data-order-price="24000">24,000</strong>원</td>
        </tr>
    </table>
</div>

<script>
    $("#btn-minus").click(function() {
        let qty = parseInt($("#qty").val()) - 1;
        $("#qty").val(qty);

        refreshOrderPrice();
    });

    $("btn-plus").click(function() {
        let qty = parseInt($("#qty").val()) + 1 ;
        $("#qty").val(qty);

        refreshOrderPrice();
    });

    function refreshOrderPrice() {
        let qty = parseInt($("#qty").val());
        let price = $("#price-txt").attr("data-price");
        let orderPrice = qty*price;

        $("#order-price-txt").attr("data-order-price", orderPrice)
            .text(orderPrice.toLocaleString());
    }
</script>
```

### 추가 버튼을 클릭했을 때 HTML 컨텐츠 추가하기

- 요구사항
  - 추가버튼을 클릭하면 경력사항 입력필드와 삭제버튼을 추가시킨다.
  - 삭제버튼을 클릭했을 때 추가된 경력사항 입력필드를 삭제한다. 
- 코딩가이드
  - 새 경력사항이 추가될 div 엘리먼트를 정의하고, 식별을 위해서 아이디를 설정한다.
  - 추가 버튼을 클릭했을 때 추가될 html 컨텐츠를 생성하고 위에서 정의한 div 엘리먼트의 자식으로 추가한다.
  - 삭제버튼을 클릭했을 새로 추가된 경력필드 항목을 전부 삭제하기 위해서 새로 추가되는 엘리먼트에 id를 설정하고, 삭제버튼에 data-xxx 속성으로 아이디값을 관리한다.
  - 삭제버튼은 미래에 추가되는 엘리먼트이기 때문에 이벤트 핸들러 등록시 주의하자.
```html
<form>
    <div>
        <label>이름</label>
        <input type="text" name="name" />
    </div>
    <div>
        <label>이메일</label>
        <input type="text" name="email" />
    </div>
    <div>
        <label>연락처</label>
        <input type="text" name="tel" />
    </div>
    <div>
        <label>경력사항</label>
        <input type="text" name="career" id="career" />
        <button type="button" id="btn-add-career">입력필드 추가</button>
        <!-- 경력사항 입력필드가 추가될 div -->
        <div id="box-career"></div>
    </div>
</form>

<script>
    let careerSeq = 1;
    $("#btn-add-career").click(function() {
        let htmlContent = `
            <div id="career-\${careerFieldSeq}">
                <input type="text" name="career" />
                <button type="button" 
                    data-career-id="#career-\${careerFieldSeq}">
                    입력필드 삭제
                </button>
            </div>
        `;

        $("#box-career").append(htmlContent);
    });

    $("#box-career").on('click', 'button', function() {
        let careerId = $(this).data('career-id');
        $(careerId).remove();
    });
</scrit>
```

### 썸네일 이미지를 클릭했을 때 큰 이미지로 표시하기
- 요구사항
  - 썸네일 이미지를 클릭하면 큰 이미지로 표시한다.
- 코딩가이드
  - 각 썸네일 이미지를 클릭했을 때 실행할 이벤트핸들러를 등록한다.
  - 클릭한 이미지가 표시될 img 엘리먼트를 식별하기 위해서 id를 설정한다.
  - 썸네일 이미지의 data-xxx 속성값에 큰이미지의 경로와 이름을 설정해둔다.
  - 썸네일 이미지를 클릭하면 미리 설정한 data-xxx 속성값을 읽어서 큰 이미지의 src 속성값을 변경한다.

```html
<div>
  <div class="box-big-img">
    <img src="큰이미지 경로 및 이름" id="big-img" />
  </div>
  <div class="box-small-img">
    <img src="작은이미지 경로 및 파일명" data-big-img-path="큰 이미지 경로 및 파일명" />
    <img src="작은이미지 경로 및 파일명" data-big-img-path="큰 이미지 경로 및 파일명" />
    <img src="작은이미지 경로 및 파일명" data-big-img-path="큰 이미지 경로 및 파일명" />
    <img src="작은이미지 경로 및 파일명" data-big-img-path="큰 이미지 경로 및 파일명" />
  </div>
<div>

<script>
  $(".box-small-img").click(function() {
    let bigImgPath = $(this).data("big-img-path");
    $("#big-img").attr("src", bigImgPath);
  });
</script>
```


## submit 이벤트 처리

### 로그인 버튼을 클릭했을 때 폼 입력값 검증하기
- 요구사항
  - 로그인 버튼을 클릭했을 때, 아이디, 비밀번호 입력필드에 입력값이 있는지 검증하고, 입력값이 없으면 경고창을 표시한다.
- 개발가이드
  - 로그인 버튼을 클릭하면 해당 form 엘리먼트에서 submit이벤트가 발생하므로, form 엘리먼트에서 발생하는 submit 이벤트를 이용하자.
  - form 엘리먼트에서 submit 이벤트가 발생하면 form은 입력값을 서버로 제출하는 것이 기본동작인데, 입력값이 유효하지 않으면 form 입력값이 서버로 제출되지 않도록 form의 기본동작 실행을 방해하자.
  - jQuery에서는 이벤트 핸들러 함수가 false를 반환하면 기본동작 실행을 방해함으로 이를 활용한다.

```html
<form id="form-login" method="post" action="/login">
    <div>
        <label>아이디</div>
        <input type="text" name="id" id="user-id" />
    </div>
    <div>
        <label>비밀번호</div>
        <input type="password" name="pwd" id="user-pwd" />
    </div>
    <div>
        <button type="submit">로그인</button>
    </div>
</form>

<script>
    $("#form-login").submit(function() {
        if ($("#user-id").val() == "") {
            alert("아이디는 필수입력값입니다.");
            return false;
        }

        if ($("#user-pwd").val() == "") {
            alert("비밀번호는 필수입력값입니다.");
            return false;
        }

        return true;
    });
</script>
```
