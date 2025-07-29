## 自由拖拽功能

### 功能目标

- 当用户在画布中拖动某一组件时，能实时更新该组件在画布中的位置属性（`top` 和 `left`)

### 实现思路

- 相关事件分析：拖拽行为实际为用户持续按下鼠标左键，并移动鼠标，最后放开鼠标的过程

- 按下鼠标：此时需要记录用户按下鼠标的位置（`startPos`），确认当前鼠标所在的组件（`currentComp`）和 `dom` 元素（`currentDom`），画布监听 `mousedown` 事件，感知拖拽开始

- 鼠标移动： 事件发生时记录当前光标位置（`currentPos`），计算与初始位置的偏移量，根据偏移量更新 `currentDom` 元素的位置信息

- 抬起鼠标：拖拽过程结束，解除 `mousedown` 事件监听

### 关键点分析

- 鼠标位置的获取：通过鼠标事件中的 `clientX` 与 `clientY` 来获取
- 需要拖动的元素的初始位置样式（`left` 和 `top`）值的获取：需要通过 `getComputedStyle` 方法来获取

附： 

1. demo 地址 [http://live.dz11.com/topic/template/dargDemo](http://live.dz11.com/topic/template/dargDemo)
2. 示例代码

``` html
 <!DOCTYPE html>
<html>
<head>
<style>
#div1 {
  border: 1px solid #ddd;
  position: absolute;
  top: 50px;
  left: 50px;
  width: 100px;
  height: 100px;
  text-align: center;
}
</style>
<script>
    window.onload = () => {
        const div1Dom = document.getElementById('div1');
        const dragInfo = {
            isDraging: false
        }
        div1.addEventListener('mousedown', e => {
            dragInfo.isDraging = true;
            dragInfo.startMousePos = {
                x: e.clientX,
                y: e.clientY,
            }
            dragInfo.startDomPos = {
                left: parseInt(window.getComputedStyle(div1Dom).left),
                top: parseInt(window.getComputedStyle(div1Dom).top),
            }
            document.addEventListener('mousemove', onMove)
        })
        const onMove = e => {
            if (dragInfo.isDraging) {
                requestAnimationFrame(() => {
                        const offSetX = e.clientX - dragInfo.startMousePos.x;
                        const offSetY = e.clientY - dragInfo.startMousePos.y;
                        div1Dom.style.top = `${dragInfo.startDomPos.top + offSetY}px`;
                        div1Dom.style.left = `${dragInfo.startDomPos.left + offSetX}px`;
                })
            }
        }
        document.addEventListener('mouseup', e => {
            dragInfo.isDraging = false; 
            document.removeEventListener('mousemove', onMove);
        })
    }
</script>
</head>
<body>

<div id="div1">div1</div>

</body>
</html>
``` 

