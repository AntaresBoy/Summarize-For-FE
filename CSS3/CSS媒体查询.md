###css3媒体查询
	css3媒体查询是响应式方案核心
	   
###媒体类型
	   all                  所有媒体（默认值）
	   screen           彩色屏幕
	   print              打印预览
	   
	   projection     手持设备
	   tv                   电视
       braille           盲文触觉设备
       embossed     盲文打印机
       speech        “听觉”类似的媒体设备
       tty                 不适用像素的设备
       

###媒体属性
	width			   （可加max min前缀）
	height			   （可加max min前缀）
	device-width	   （可加max min前缀）
	device-pixel-ratio（可加max min前缀，需要加webkit前缀）
	orientation   portrait竖屏
				  landscape横屏

###操作符，关键字 (only,and,(，or),not)
	   		only：
	   			防止老旧的浏览器  不支持带媒体属性的查询而应用到给定的样式.
	   			@media only screen and (min-width:800px ){
	   								规则；
	   								规则
	   			}
	   			@media  screen and (min-width:800px ){
	   								规则；
	   								规则
	   			}
	   			在老款的浏览器下
	   				@media only    --->    因为没有only这种设备 规则被忽略
	   				@media screen --->   因为有screen这种设备而且老浏览器会忽略带媒体属性的查询
	   			
	   			建议在每次抒写media query的时候带上only
	   	
	   		and:
	   			连接媒体属性 、连接媒体类型
	   			对于所有的连接选项都要匹配成功才能应用规则
	   		
	   		or(,) : 和and相似
	   			对于所有的连接选项只要匹配成功一个就能应用规则
	   		
	   		not:取反

