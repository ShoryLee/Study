# 搜索框输入内容实时显示		
* 或许能解决chrome浏览器在执行ajax时，中文输入未完成状态也实时请求
* 使用timeout超时延迟执行
* 中英文区分处理

```javascript
$(document).ready(function(){
	var searchId = "#search-me";
	var showText = "#show";
	//中文判断
	var myReg = /^[\u4e00-\u9fa5]+$/;
	//设置搜索超时，300ms后执行一次，减轻http负担
	var lastTime = 0;
	
    //绑定keyup和focus事件，keyup用于英文输入，focus进行中文输入
    $(searchId).bind('keyup', 'focus', function(event){
		var searchValue = $.trim($(searchId).val());
      	//如果输入为空，包括空格，就不执行
      	if (searchValue != '') {
        	//获取时间戳
        	lastTime = event.timeStamp;
        	//将操作都放置于超时函数内				
        	setTimeout(function(){
				if(lastTime - event.timeStamp == 0){
          		//进行汉字判断
            	if (myReg.test(searchValue)){
              		//如果是汉字，当键盘按下空格32或删除键8的时候，再执行
            		if(event.which == 8 || event.which == 32){
						$(showText).html(searchValue);
						// alert (searchValue);
						}
					} else {
              			//英文，直接执行
              			$(showText).html(searchValue);
						// alert (searchValue);
						}
					}
				}, 300);  //超时300ms					
			} else {
        		//此处用于清空输入，否则显示的文本将会在输入框删除完之后残留最后一个字符
          		$(showText).html('');
				}
		  });
	});
```	
