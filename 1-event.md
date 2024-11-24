# 이벤트 처리

## 클릭 이벤트 처리

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
