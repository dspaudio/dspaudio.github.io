두 가지 코드 및 화면 디자인은 HTML, CSS, javascript, 포토샵 작업을 홀로 진행했습니다.

# Miceseoul 유니크베뉴
<http://kr.miceseoul.com/venue/>

조건별 결과 실시간 반영 및 지도에 표시, 지도를 클릭하면 해당 조건에 맞는 지역만 강조 표시(현재는 DB에 결과값의 설정이 잘못되어 있는 상황으로 보여 제대로 표시 되지 않습니다.)

* 왼쪽 조건 설정 부분

```{.html}
<div class="uv_browse_left">
  <dl class="uv_browse">
    <dt class="uv_browse_title">시설</dt>
    <dd class="uv_browse_items">
      <ul class="uv_browse_items_list">
        <li><input type="checkbox" class="uv_browse_fac" id="ft1" value="Riverside">&nbsp;<label for="ft1">수변 시설</label></li>
        <li><input type="checkbox" class="uv_browse_fac" id="ft2" value="Traditional">&nbsp;<label for="ft2">전통 시설</label></li>
        <li><input type="checkbox" class="uv_browse_fac" id="ft3" value="Modern">&nbsp;<label for="ft3">현대(모던) 시설</label></li>
        <li><input type="checkbox" class="uv_browse_fac" id="ft4" value="Performance">&nbsp;<label for="ft4">공연&middot;문화 시설</label></li>
        <li><input type="checkbox" class="uv_browse_fac" id="ft5" value="Experience">&nbsp;<label for="ft5">체험 시설</label></li>
        <li><input type="checkbox" class="uv_browse_fac" id="ft6" value="Museum">&nbsp;<label for="ft6">박물관&middot;미술관</label></li>
      </ul>
    </dd>
  </dl>
  <dl class="uv_browse">
    <dt class="uv_browse_title">수용인원</dt>
    <dd class="uv_browse_items">
      <ul class="uv_browse_items_list">
        <li><input type="radio" name="uv_browse_cap" class="uv_browse_cap uv_browse_cap2" id="sc0" >&nbsp;<label for="sc0">전체 보기</label></li>
        <li><input type="radio" name="uv_browse_cap" class="uv_browse_cap" id="sc1" value="1">&nbsp;<label for="sc1">0-50</label></li>
        <li><input type="radio" name="uv_browse_cap" class="uv_browse_cap" id="sc2" value="2">&nbsp;<label for="sc2">51-100</label></li>
        <li><input type="radio" name="uv_browse_cap" class="uv_browse_cap" id="sc3" value="3">&nbsp;<label for="sc3">101-150</label></li>
        <li><input type="radio" name="uv_browse_cap" class="uv_browse_cap" id="sc4" value="4">&nbsp;<label for="sc4">151-300</label></li>
        <li><input type="radio" name="uv_browse_cap" class="uv_browse_cap" id="sc5" value="5">&nbsp;<label for="sc5">301-</label></li>
      </ul>
    </dd>
  </dl>
  <a href="/venue/서울유니크베뉴 개요.xlsx" class="uv_overview_title" target="_blank">서울유니크베뉴 개요</a>
</div>
```

왼쪽 조건 설정 부분에서 값을 가져와 해당 값을 css class로 지정하여 해당되는 결과를 표시하도록 했습니다. 결과를 표시하는 부분에는 각 조건을 css class로 할당하여 해당 조건에 맞는 결과 값을 복수 체크해서 조건에 해당되지 않는 것만 숨기도록 설정했습니다.

* 결과 표시 부분

```{.html}
<ul class="uv_menu_list">
  <li class="uv_list uv_list_cap_2 uv_list_Riverside uv_list_center"><span>01</span><a href="./venue_view.php?uv_uid=3" title="클릭하면 700 요트클럽의 상세 정보를 볼 수 있습니다">700 요트클럽</a></li>
  <li class="uv_list uv_list_cap_4 uv_list_Riverside uv_list_east"><span>02</span><a href="./venue_view.php?uv_uid=22" title="클릭하면 애스톤하우스의 상세 정보를 볼 수 있습니다”>애스톤하우스</a></li>

……

</ul>
```

* javascript

```{.javascript}
$(document).ready(function(){
  function check_facility() {
    var fac=[];
    for ( var i=0; i < $(".uv_browse_fac").length; i++) {
      if ($(".uv_browse_fac").eq(i).prop("checked")){
        fac.push($(".uv_browse_fac").eq(i).val());
      }
    }
    if (fac.length == 0) {
      $(".uv_map_num").addClass("fshow");
      $(".uv_list").addClass("fshow");
    } else {
      $(".uv_map_num").removeClass("fshow");
      $(".uv_list").removeClass("fshow");
    }
    var total = $(".uv_map_num").length;
    for ( var i2 =0; i2 < total ; i2++ ){
      for (var fi = 0; fi < fac.length; fi++){
        if ($(".uv_map_num").eq(i2).hasClass("uv_map_"+fac[fi])){
          $(".uv_map_num").eq(i2).addClass("fshow");
          $(".uv_list").eq(i2).addClass("fshow");
        }
      }
    }
  }
  function check_capacity() {
    var cap=[];
    for ( var ci=0; ci < $(".uv_browse_cap").length; ci++) {
      if ($(".uv_browse_cap").eq(ci).prop("checked")){
        cap.push($(".uv_browse_cap").eq(ci).val());
      }
    }
    if (cap.length == 0) {
      $(".uv_map_num").addClass("cshow");
      $(".uv_list").addClass("cshow");
    } else {
      $(".uv_map_num").removeClass("cshow");
      $(".uv_list").removeClass("cshow");
    }
    var total = $(".uv_map_num").length;
    for ( var i2 =0; i2 < total ; i2++ ){
      for (var ci2 = 0; ci2 < cap.length; ci2++){
        if ($(".uv_map_num").eq(i2).hasClass("uv_map_cap_"+cap[ci2])){
          $(".uv_map_num").eq(i2).addClass("cshow");
          $(".uv_list").eq(i2).addClass("cshow");
        }
      }
    }
  }
  function map_num_show() {
    var total = $(".uv_map_num").length;
    for ( var i=0; i < total ; i++) {
      if ($(".uv_map_num").eq(i).hasClass("lshow") && $(".uv_map_num").eq(i).hasClass("fshow") && $(".uv_map_num").eq(i).hasClass("cshow")) {
        $(".uv_map_num").eq(i).addClass("show");
        $(".uv_list").eq(i).addClass("show");
      } else {
        $(".uv_map_num").eq(i).removeClass("show");
        $(".uv_list").eq(i).removeClass("show");
      }
    }
    $(".show").show();
  }

  $(".uv_map_num").addClass("lshow").addClass("fshow").addClass("cshow");
  $(".uv_list").addClass("lshow").addClass("fshow").addClass("cshow");
  map_num_show();

  $(".img_map_area").click(function(e){
    var loc = $(this).data("loc");
    var total = $(".uv_map_num").length;
    $(".uv_map_num").removeClass("lshow");
    $(".uv_list").removeClass("lshow");
    for ( var i =0; i < total ; i++ ){
      if ($(".uv_map_num").eq(i).hasClass("uv_map_"+loc)){
        $(".uv_map_num").eq(i).addClass("lshow");
        $(".uv_list").eq(i).addClass("lshow");
      }
    }
    check_facility();
    check_capacity();
    map_num_show();

    $(".uv_map_help_text").fadeIn();
    $("#uv_map_"+loc).fadeIn();
    $(".uv_map").fadeOut();
    e.preventDefault();
  });
  $(".uv_browse_fac").click(function(){
    check_facility();
    check_capacity();
    map_num_show();
  });
  $(".uv_browse_cap").click(function(){
    $('.uv_browse_cap2').removeProp('checked');
    check_facility();
    check_capacity();
    map_num_show();
  });
  $(".uv_map_focused").click(function(){
    $(".uv_map_help_text").fadeOut();
    $(".uv_map_focused").hide();
    $(".uv_map").show();
    $(".uv_map_num").addClass("lshow");
    $(".uv_list").addClass("lshow");
    map_num_show();
    check_facility();
    check_capacity();
    map_num_show();
  });
  $(".uv_map_num").hover(function(){
    var info_id = $(this).text();
    $(".uv_num").text(info_id);
    $(".uv_map_hover_info").load("/venue/venue_info.php?uv_num="+info_id,function(){ $(".uv_map_hover_info").fadeIn("fast"); });
  },function(){
    $(".uv_map_hover_info").hide();
  });
});
```

#대명리조트 랜딩 페이지
<http://efrontier.kr/dmeventu/>
로딩 단계에서 PC, 모바일을 PHP로 체크하고 해당 환경에 맞는 페이지를 로드, 표시합니다. PC와 모바일의 레이아웃과 디자인이 달라 이런 방식으로 구현했습니다.

* 오른쪽 가이드
```{.html}
<div id="right-guide">
  <a id="guide1" class="guide active-guide" href="#"></a>
  <a id="guide2" class="guide" href="#p2"></a>
  <a id="guide3" class="guide" href="#p3"></a>
  <a id="guide4" class="guide" href="#p4"></a>
  <a id="guide5" class="guide" href="#p5"></a>
  <a id="guide6" class="guide" href="#p6"></a>
</div>
```

오른쪽 가이드를 클릭하면 화면상의 해당 위치로 이동하고, 오른쪽 가이드에도 이동된 위치를 표시하도록 구현했습니다.

* javascript
```{.javascript}
$(document).ready(function(){
  var lastId,
    header = $(".header"),
    headerHeight = header.outerHeight(),
    menuItems = $("#right-guide").find("a"),
    scrollItems = menuItems.map(function(){
      var item = $($(this).attr("href"));
      if (item.length) { return item; }
    });
  menuItems.click(function(e){
    var href = $(this).attr("href"),
        offsetTop = href === "#" ? 0 : $(href).offset().top-headerHeight;
    $('html, body').stop().animate({
        scrollTop: offsetTop
    }, 300);
    $(this).blur();
    e.preventDefault();
  });

  var $sidebar = $("#rightbar_scroll"),
  offset = $sidebar.offset(),
  topPadding = 140;

  $(window).scroll(function(){
    var fromTop = $(this).scrollTop()+headerHeight+1;
    var cur = scrollItems.map(function(){
      if ($(this).offset().top < fromTop)
        return this;
    });
    cur = cur[cur.length-1];
    var id = cur && cur.length ? cur[0].id : "";
    if (lastId !== id) {
      lastId = id;
      menuItems
      .removeClass("active-guide")
      .filter("[href=#"+id+"]").addClass("active-guide");
    }

    if ($(window).scrollTop() > offset.top) {
      $sidebar.stop().animate({
        marginTop: $(window).scrollTop() - offset.top + topPadding
      },200);
    } else {
      $sidebar.stop().animate({
        marginTop: 0
      },200);
    }
  });
});
```

