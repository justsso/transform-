# transform-
在移动端实现滑屏功能,如幻灯片,页面滚动的方式有定位法,transform法,从性能角度考虑,transform的方法就是上策.

然而,在元素上直接获取transform样式,返回的是矩阵形式的,由得到的矩阵再反推tranfrom的各种值,我的天....果断放弃之~,可以给元素挂在一个对象,element.transform{},我们给元素添加的transform属性就可以存在这个对象中,如:translateX,translateY,translateZ,rotateX,rotateY,skew等等

```
function transform( ele, attr, val ){
				if (!ele.transform) {
					ele.transform = {};
				}
				if ( typeof  val == "undefined") {
					//获取transform
					return ele.transform[attr];
				}
				//设置transform
				var inner = "";
				ele.transform[attr] = val;
				for( var s in ele.transform){
					switch (s){
						case 'translateX':
						case 'translateY':
						case 'translateZ':
							inner +=' '+s+'('+ele.transform[s] +'px)';
							break;
						case 'rotateX':
						case 'rotateY':
						case 'rotateZ':
						case 'skewX':
						case 'skewY':
							inner += " "+s+"("+ ele.transform[s]+"deg)";
							break;
						case 'scaleX':
						case 'scaleY':
						case 'scale':
							inner +=' '+ s +'('+ele.transform[s]+')';
					}
				}
				ele.style.WebkitTransform = ele.style.transform = inner;
			}
```
如果传入了第三个参数,那就是设置这个元素的tranform的某个属性;如果没有传入第三个参数,那就是获取这个元素的transform属性
**注意:**
在操作之前,先transform(ele,"translateZ",0);这是为了触发移动端的3d加载;然后transform(ele,"translateX",0);transform( ele, "tranlateY",0);这是因为,后面可能会获取元素的transform属性,如果一开始没有赋值,返回的是"undefined"...**谨记**

