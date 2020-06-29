1.二者定义  
  element.offsetLeft:元素的左外边框至包含元素的左内边框之间的像素距离  
  element.style.left:定位元素左外边距边界与其包含块左边界之间的偏移（来自w3school）  
  如果父div的position定义为relative,子div的position定义为absolute,那么子div的style.left的值是相对于父div的值，这同offsetLeft是相同的  

2.二者区别  
  a.element.style.left可读可写，element.offsetLeft只读；所以如果要改变div的位置只能使用element.style.left  
  b.作为可读属性，element.style.left只能获取行内样式，写在style标签和css文件内的left属性是无法通过element.style.left获得的；这个时候可以通过element.offsetLeft来获取  
  c.作为可读属性，element.style.left返回字符串带“px”如28px，element.offsetLeft返回数值28；如果要进行数值运算用element.offsetLeft  
  d.通过element.style.left设置的值如8.22px，element.style.left获取的值是字符串8.22px，element.offsetLeft获取的是四舍五入之后的数值8  
  
3.二者转换  
  element.style.left = element.offsetLeft + 'px';  
